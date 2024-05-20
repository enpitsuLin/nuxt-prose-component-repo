<script setup lang="ts">
import { joinURL, withLeadingSlash, withTrailingSlash } from 'ufo'
import { computed, useRuntimeConfig } from '#imports'

defineOptions({ inheritAttrs: false })

const props = defineProps<{
  src: string
  alt?: string
  width?: string | number
  height?: string | number
}>()

const refinedSrc = computed(() => {
  if (props.src?.startsWith('/') && !props.src.startsWith('//')) {
    const _base = withLeadingSlash(withTrailingSlash(useRuntimeConfig().app.baseURL))
    if (_base !== '/' && !props.src.startsWith(_base))
      return joinURL(_base, props.src)
  }
  return props.src
})
</script>

<template>
  <figure>
    <img :src="refinedSrc" :alt="alt" :width="width" :height="height">
    <figcaption v-if="alt">
      <hr>
      <span>{{ alt }}</span>
    </figcaption>
  </figure>
</template>
