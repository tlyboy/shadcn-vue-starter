<script setup lang="ts">
import { useColorMode } from '@vueuse/core'
import { nextTick } from 'vue'
import { Button } from '@/components/ui/button'

const color = useColorMode()

function toggleDark(event: MouseEvent) {
  const isAppearanceTransition = !window.matchMedia(
    '(prefers-reduced-motion: reduce)',
  ).matches

  if (
    !isAppearanceTransition ||
    typeof document.startViewTransition !== 'function'
  ) {
    color.value = color.value === 'dark' ? 'light' : 'dark'
    return
  }

  const x = event.clientX
  const y = event.clientY
  const endRadius = Math.hypot(
    Math.max(x, innerWidth - x),
    Math.max(y, innerHeight - y),
  )

  const transition = document.startViewTransition(async () => {
    color.value = color.value === 'dark' ? 'light' : 'dark'
    await nextTick()
  })
  transition.ready.then(() => {
    const clipPath = [
      `circle(0px at ${x}px ${y}px)`,
      `circle(${endRadius}px at ${x}px ${y}px)`,
    ]
    document.documentElement.animate(
      {
        clipPath: color.value === 'dark' ? [...clipPath].reverse() : clipPath,
      },
      {
        duration: 400,
        easing: 'ease-out',
        pseudoElement:
          color.value === 'dark'
            ? '::view-transition-old(root)'
            : '::view-transition-new(root)',
      },
    )
  })
}
</script>

<template>
  <Button
    variant="secondary"
    size="icon"
    class="size-8"
    title="切换深色模式"
    @click="toggleDark"
  >
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      strokeWidth="2"
      strokeLinecap="round"
      strokeLinejoin="round"
      class="size-4.5"
    >
      <path stroke="none" d="M0 0h24v24H0z" fill="none" />
      <path d="M12 12m-9 0a9 9 0 1 0 18 0a9 9 0 1 0 -18 0" />
      <path d="M12 3l0 18" />
      <path d="M12 9l4.65 -4.65" />
      <path d="M12 14.3l7.37 -7.37" />
      <path d="M12 19.6l8.85 -8.85" />
    </svg>
  </Button>
</template>
