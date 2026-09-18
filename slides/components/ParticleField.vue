<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

/**
 * The deck's signature visual: a field of tiny outlined triangles in the brand's
 * chromatic spectrum, drifting slowly against the void.
 *
 * Canvas 2D rather than WebGL. The field is thousands of 1px strokes with no
 * shading, no blending, and no camera, so a shader would add a dependency and a
 * compile step to draw something the 2D context already draws cheaply.
 */
const props = withDefaults(
  defineProps<{
    /** cloud: dense organic cluster. ambient: sparse drift. lanes: three bands. */
    variant?: 'cloud' | 'ambient' | 'lanes'
    /** Multiplier on the variant's base particle count. */
    density?: number
    /** Overall opacity of the whole field. */
    opacity?: number
  }>(),
  { variant: 'ambient', density: 1, opacity: 1 },
)

const canvas = ref<HTMLCanvasElement | null>(null)
let frame = 0

const PALETTE = ['#8052ff', '#ffb829', '#15846e', '#b07cff', '#5b8dff', '#ff6bd6']

interface Particle {
  x: number
  y: number
  size: number
  color: string
  alpha: number
  rotation: number
  spin: number
  driftX: number
  driftY: number
}

/** Organic blob radius: a circle perturbed by two sine terms so the edge never reads as geometry. */
function blobRadius(angle: number, base: number): number {
  return base * (0.72 + 0.2 * Math.sin(angle * 3) + 0.12 * Math.sin(angle * 5 + 1.4))
}

function build(width: number, height: number): Particle[] {
  const counts = { cloud: 2200, ambient: 420, lanes: 1400 }
  const total = Math.round(counts[props.variant] * props.density)
  const particles: Particle[] = []

  for (let i = 0; i < total; i += 1) {
    let x: number
    let y: number

    if (props.variant === 'cloud') {
      // Concentrate toward the centre: sqrt on a uniform sample would spread
      // evenly by area, so the exponent is raised to pull mass inward.
      const angle = Math.random() * Math.PI * 2
      const reach = Math.pow(Math.random(), 0.55)
      const radius = blobRadius(angle, Math.min(width, height) * 0.46) * reach
      x = width / 2 + Math.cos(angle) * radius
      y = height / 2 + Math.sin(angle) * radius * 0.86
    } else if (props.variant === 'lanes') {
      const lane = i % 3
      const laneCentre = height * (0.26 + lane * 0.24)
      x = Math.random() * width
      y = laneCentre + (Math.random() - 0.5) * height * 0.13
    } else {
      x = Math.random() * width
      y = Math.random() * height
    }

    const edgeFade = props.variant === 'cloud' ? 1 : 0.55
    particles.push({
      x,
      y,
      size: 3 + Math.random() * 6,
      color: PALETTE[Math.floor(Math.random() * PALETTE.length)] ?? '#8052ff',
      alpha: (0.25 + Math.random() * 0.6) * edgeFade,
      rotation: Math.random() * Math.PI * 2,
      spin: (Math.random() - 0.5) * 0.004,
      driftX: (Math.random() - 0.5) * 0.12,
      driftY: (Math.random() - 0.5) * 0.12,
    })
  }
  return particles
}

onMounted(() => {
  const el = canvas.value
  if (el === null) {
    return
  }
  const ctx = el.getContext('2d')
  if (ctx === null) {
    return
  }

  const dpr = Math.min(window.devicePixelRatio || 1, 2)
  const width = el.clientWidth
  const height = el.clientHeight
  el.width = width * dpr
  el.height = height * dpr
  ctx.scale(dpr, dpr)

  const particles = build(width, height)

  const draw = (): void => {
    ctx.clearRect(0, 0, width, height)
    ctx.lineWidth = 1

    for (const p of particles) {
      p.x += p.driftX
      p.y += p.driftY
      p.rotation += p.spin

      // Wrap rather than respawn, so density stays constant over a long talk.
      if (p.x < -20) p.x = width + 20
      if (p.x > width + 20) p.x = -20
      if (p.y < -20) p.y = height + 20
      if (p.y > height + 20) p.y = -20

      ctx.save()
      ctx.translate(p.x, p.y)
      ctx.rotate(p.rotation)
      ctx.globalAlpha = p.alpha * props.opacity
      ctx.strokeStyle = p.color
      ctx.beginPath()
      ctx.moveTo(0, -p.size / 2)
      ctx.lineTo(p.size / 2, p.size / 2)
      ctx.lineTo(-p.size / 2, p.size / 2)
      ctx.closePath()
      ctx.stroke()
      ctx.restore()
    }
    frame = requestAnimationFrame(draw)
  }
  draw()
})

onBeforeUnmount(() => {
  cancelAnimationFrame(frame)
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
