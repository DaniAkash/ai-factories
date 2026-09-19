<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

/**
 * The deck's signature visual: a field of tiny outlined triangles in the brand's
 * chromatic spectrum, against the void.
 *
 * The `cloud` variant opens as a tight circle and slowly scatters outward, so
 * landing on a hero slide blooms the constellation rather than showing a static
 * field. The scatter runs on an ease-out curve and then stops completely, which
 * is both the nicer motion and the cheaper one.
 *
 * Performance is otherwise the whole design of this component. A full-bleed
 * canvas repainting every frame costs a 1920x1080 layer repaint plus a GPU
 * texture upload, and Slidev keeps neighbouring slides mounted, so a naive
 * version runs several of those at once for slides nobody is looking at. See
 * design.md for the five rules that keep it cheap.
 */
const props = withDefaults(
  defineProps<{
    variant?: 'cloud' | 'ambient' | 'lanes'
    density?: number
    opacity?: number
    /** Circle centre as a fraction of the canvas. Cloud only. */
    cx?: number
    cy?: number
  }>(),
  { variant: 'ambient', density: 1, opacity: 1, cx: 0.73, cy: 0.5 },
)

const canvas = ref<HTMLCanvasElement | null>(null)
let frame = 0
let observer: IntersectionObserver | null = null

/* Darkened for the light ground: these are strokes at partial alpha, so a tint
   that reads on black disappears entirely on paper. */
const PALETTE = ['#6a35f5', '#9a5b00', '#0f6d5a', '#7a4fd8', '#2f5fd0', '#c02f90']

const RENDER_SCALE = 0.6
const ALPHA_STEPS = 4

/**
 * How far the circle opens out, and over how long.
 *
 * Kept deliberately modest. Past roughly 1.4 the field stops reading as a cloud
 * placed beside the headline and becomes a full-bleed wash that clips at the
 * canvas edges, which loses the composition the slide is built around.
 */
const SPREAD_TO = 1.3
const SPREAD_MS = 30_000

interface Particle {
  /** Static variants position directly. */
  x: number
  y: number
  /** Cloud positions from polar coordinates so the whole field can expand together. */
  cos: number
  sin: number
  baseRadius: number
  driftX: number
  driftY: number
  size: number
}

type Bucket = { style: string; items: Particle[] }

function hexToRgba(hex: string, alpha: number): string {
  const n = Number.parseInt(hex.slice(1), 16)
  return `rgba(${(n >> 16) & 255}, ${(n >> 8) & 255}, ${n & 255}, ${alpha})`
}

function build(width: number, height: number): Bucket[] {
  const counts = { cloud: 900, ambient: 220, lanes: 600 }
  const total = Math.round(counts[props.variant] * props.density)
  const buckets = new Map<string, Bucket>()
  // A circle that clears the headline column rather than filling the canvas.
  const circleRadius = Math.min(width, height) * 0.32

  for (let i = 0; i < total; i += 1) {
    let x = 0
    let y = 0
    let cos = 0
    let sin = 0
    let baseRadius = 0

    if (props.variant === 'cloud') {
      const angle = Math.random() * Math.PI * 2
      // The exponent biases mass toward the centre; a plain sqrt would spread
      // it evenly by area and the circle would read as a ring.
      baseRadius = circleRadius * Math.pow(Math.random(), 0.62)
      cos = Math.cos(angle)
      sin = Math.sin(angle)
    } else if (props.variant === 'lanes') {
      const laneCentre = height * (0.26 + (i % 3) * 0.24)
      x = Math.random() * width
      y = laneCentre + (Math.random() - 0.5) * height * 0.13
    } else {
      x = Math.random() * width
      y = Math.random() * height
    }

    const colour = PALETTE[Math.floor(Math.random() * PALETTE.length)] ?? '#6a35f5'
    const edgeFade = props.variant === 'cloud' ? 1 : 0.55
    const rawAlpha = (0.25 + Math.random() * 0.6) * edgeFade * props.opacity
    const step = Math.max(1, Math.round(rawAlpha * ALPHA_STEPS))
    const style = hexToRgba(colour, (step / ALPHA_STEPS) * 0.85)

    const bucket = buckets.get(style) ?? { style, items: [] }
    bucket.items.push({
      x,
      y,
      cos,
      sin,
      baseRadius,
      driftX: (Math.random() - 0.5) * 3.5,
      driftY: (Math.random() - 0.5) * 3.5,
      size: 3 + Math.random() * 6,
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
  const isCloud = props.variant === 'cloud'
  const animated = isCloud && !reduceMotion
  const MIN_FRAME_MS = 1000 / 30

  let buckets: Bucket[] = []
  let width = 0
  let height = 0
  let centreX = 0
  let centreY = 0
  let ready = false
  let running = false
  let last = 0
  let startedAt = 0

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
    centreX = width * props.cx
    centreY = height * props.cy
    buckets = build(width, height)
    ready = true
    return true
  }

  /** `spread` of 1 is the closed circle; it eases out toward SPREAD_TO. */
  const paint = (spread: number): void => {
    ctx.clearRect(0, 0, width, height)
    ctx.lineWidth = 1
    const scatter = spread - 1
    for (const bucket of buckets) {
      ctx.strokeStyle = bucket.style
      ctx.beginPath()
      for (const p of bucket.items) {
        const px = isCloud
          ? centreX + p.cos * p.baseRadius * spread + p.driftX * scatter
          : p.x
        const py = isCloud
          ? centreY + p.sin * p.baseRadius * spread + p.driftY * scatter
          : p.y
        const half = p.size / 2
        ctx.moveTo(px, py - half)
        ctx.lineTo(px + half, py + half)
        ctx.lineTo(px - half, py + half)
        ctx.closePath()
      }
      ctx.stroke()
    }
  }

  const step = (now: number): void => {
    if (!running) {
      return
    }
    if (now - last < MIN_FRAME_MS) {
      frame = requestAnimationFrame(step)
      return
    }
    last = now

    const t = Math.min(1, (now - startedAt) / SPREAD_MS)
    const eased = 1 - Math.pow(1 - t, 3)
    paint(1 + (SPREAD_TO - 1) * eased)

    if (t >= 1) {
      // Settled. Nothing left to animate, so stop rather than repaint a still.
      running = false
      return
    }
    frame = requestAnimationFrame(step)
  }

  observer = new IntersectionObserver((entries) => {
    const visible = entries.some((entry) => entry.isIntersecting)
    if (visible) {
      if (!ready && !initialise()) {
        return
      }
      if (!animated) {
        paint(isCloud ? SPREAD_TO : 1)
        return
      }
      if (!running) {
        // Re-entering a hero slide replays the bloom.
        running = true
        startedAt = performance.now()
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
