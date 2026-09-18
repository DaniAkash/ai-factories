<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'

const { $slidev, $page } = useSlideContext()
const total = computed(() => $slidev?.nav?.total ?? 0)
const current = computed(() => $page?.value ?? 0)
const label = computed(() => `${String(current.value).padStart(2, '0')} / ${total.value}`)
</script>

<template>
  <div>{{ label }}</div>
</template>
