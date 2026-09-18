<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

/**
 * The deck's signature visual: a field of tiny outlined triangles in the brand's
 * chromatic spectrum, against the void.
 *
 * Performance is the whole design of this component. A full-bleed canvas
 * repainting every frame costs a 1920x1080 layer repaint plus a GPU texture
 * upload, and Slidev keeps neighbouring slides mounted, so a naive version runs
 * several of those at once including for slides nobody is looking at. Three
 * rules keep it cheap:
 *
 * 1. Only `cloud` animates. `ambient` and `lanes` paint a single frame and stop,
 *    because a sparse field drifting at 0.1px per frame is not perceptible in a
 *    talk and does not justify a permanent repaint.
 * 2. Animation pauses whenever the canvas is off screen, which covers both the
 *    slides Slidev keeps mounted either side of the current one and a
 *    backgrounded tab.
 * 3. Strokes are batched per colour bucket, so a frame issues a couple of dozen
 *    stroke calls instead of one per particle.
 */
const props = withDefaults(
  defineProps<{
    variant?: 'cloud' | 'ambient' | 'lanes'
    density?: number
    opacity?: number
  }>(),
  { variant: 'ambient', density: 1, opacity: 1 },
)

const canvas = ref<HTMLCanvasElement | null>(null)
let frame = 0
let observer: IntersectionObserver | null = null

const PALETTE = ['#8052ff', '#ffb829', '#15846e', '#b07cff', '#5b8dff', '#ff6bd6']

/**
 * The field is decoration, so it renders below CSS resolution and is scaled up.
 * At a 1px stroke the difference is invisible and it cuts the painted pixel
 * count by roughly two thirds.
 */
const RENDER_SCALE = 0.6

/** Alpha is quantised so particles can share a stroke batch. */
const ALPHA_STEPS = 4

interface Particle {
  x: number
  y: number
  size: number
  driftX: number
  driftY: number
}

type Bucket = { style: string; items: Particle[] }

function blobRadius(angle: number, base: number): number {
  return base * (0.72 + 0.2 * Math.sin(angle * 3) + 0.12 * Math.sin(angle * 5 + 1.4))
}

function hexToRgba(hex: string, alpha: number): string {
  const n = Number.parseInt(hex.slice(1), 16)
  return `rgba(${(n >> 16) & 255}, ${(n >> 8) & 255}, ${n & 255}, ${alpha})`
}

function build(width: number, height: number): Bucket[] {
  const counts = { cloud: 900, ambient: 220, lanes: 600 }
  const total = Math.round(counts[props.variant] * props.density)
  const buckets = new Map<string, Bucket>()

  for (let i = 0; i < total; i += 1) {
    let x: number
    let y: number

    if (props.variant === 'cloud') {
      const angle = Math.random() * Math.PI * 2
      const reach = Math.pow(Math.random(), 0.55)
      const radius = blobRadius(angle, Math.min(width, height) * 0.46) * reach
      x = width / 2 + Math.cos(angle) * radius
      y = height / 2 + Math.sin(angle) * radius * 0.86
    } else if (props.variant === 'lanes') {
      const laneCentre = height * (0.26 + (i % 3) * 0.24)
      x = Math.random() * width
      y = laneCentre + (Math.random() - 0.5) * height * 0.13
    } else {
      x = Math.random() * width
      y = Math.random() * height
    }

    const colour = PALETTE[Math.floor(Math.random() * PALETTE.length)] ?? '#8052ff'
    const edgeFade = props.variant === 'cloud' ? 1 : 0.55
    const rawAlpha = (0.25 + Math.random() * 0.6) * edgeFade * props.opacity
    const step = Math.max(1, Math.round(rawAlpha * ALPHA_STEPS))
    const style = hexToRgba(colour, (step / ALPHA_STEPS) * 0.85)

    const bucket = buckets.get(style) ?? { style, items: [] }
    bucket.items.push({
      x,
      y,
      size: 3 + Math.random() * 6,
      driftX: (Math.random() - 0.5) * 0.12,
      driftY: (Math.random() - 0.5) * 0.12,
    })
    buckets.set(style, bucket)
  }
  return [...buckets.values()]
}

onMounted(() => {
  const el = canvas.value
  if (el === null) {
    return
  }
  const ctx = el.getContext('2d', { alpha: true })
  if (ctx === null) {
    return
  }

  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  const animated = props.variant === 'cloud' && !reduceMotion
  const MIN_FRAME_MS = 1000 / 30

  let buckets: Bucket[] = []
  let width = 0
  let height = 0
  let ready = false
  let running = false
  let last = 0

  /**
   * Sizing is deferred until the canvas is actually on screen. Slidev mounts
   * neighbouring slides hidden, where clientWidth is 0, so measuring at mount
   * would fix the backing store at 0x0 and the field would never paint.
   */
  const initialise = (): boolean => {
    const cssWidth = el.clientWidth
    const cssHeight = el.clientHeight
    if (cssWidth === 0 || cssHeight === 0) {
      return false
    }
    width = Math.round(cssWidth * RENDER_SCALE)
    height = Math.round(cssHeight * RENDER_SCALE)
    el.width = width
    el.height = height
    buckets = build(width, height)
    ready = true
    return true
  }

  const paint = (): void => {
    ctx.clearRect(0, 0, width, height)
    ctx.lineWidth = 1
    for (const bucket of buckets) {
      ctx.strokeStyle = bucket.style
      ctx.beginPath()
      for (const p of bucket.items) {
        const half = p.size / 2
        ctx.moveTo(p.x, p.y - half)
        ctx.lineTo(p.x + half, p.y + half)
        ctx.lineTo(p.x - half, p.y + half)
        ctx.closePath()
      }
      ctx.stroke()
    }
  }

  const step = (now: number): void => {
    if (!running) {
      return
    }
    frame = requestAnimationFrame(step)
    if (now - last < MIN_FRAME_MS) {
      return
    }
    last = now
    for (const bucket of buckets) {
      for (const p of bucket.items) {
        p.x += p.driftX
        p.y += p.driftY
        if (p.x < -20) p.x = width + 20
        if (p.x > width + 20) p.x = -20
        if (p.y < -20) p.y = height + 20
        if (p.y > height + 20) p.y = -20
      }
    }
    paint()
  }

  observer = new IntersectionObserver((entries) => {
    const visible = entries.some((entry) => entry.isIntersecting)
    if (visible) {
      if (!ready && !initialise()) {
        return
      }
      paint()
      if (animated && !running) {
        running = true
        last = 0
        frame = requestAnimationFrame(step)
      }
      return
    }
    if (running) {
      running = false
      cancelAnimationFrame(frame)
    }
  })
  observer.observe(el)
})

onBeforeUnmount(() => {
  cancelAnimationFrame(frame)
  observer?.disconnect()
  observer = null
})
</script>

<template>
  <canvas ref="canvas" class="particle-field" />
</template>

<style scoped>
.particle-field {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  pointer-events: none;
}
</style>
