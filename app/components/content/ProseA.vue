<template>
  <NuxtLink
    :href="formattedHref"
    :target="props.target"
  >
    <slot />
  </NuxtLink>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import type { PropType } from 'vue'

const props = defineProps({
  href: {
    type: String,
    default: '',
  },
  target: {
    type: String as PropType<'_blank' | '_parent' | '_self' | '_top' | (string & object) | null | undefined>,
    default: undefined,
    required: false,
  },
})

// パスを変換する算出プロパティ
const formattedHref = computed(() => {
  let url = props.href

  // 1. `/content/dict/` を `/content/` に変換する（先頭の有無に対応）
  url = url.replace(/^\/?content\/dict\//, '/content/')

  // 2. 「index.md」の記述を完全に削除する
  // 例: /content/tech/index.md -> /content/tech/
  // 例: content/tech/index.md  -> /content/tech/
  if (url.endsWith('index.md')) {
    url = url.replace(/index\.md$/, '')
  }

  // 3. 先頭が `/` で始まっていない場合は、ルート相対パスになるよう `/` を付与
  if (!url.startsWith('/') && !url.startsWith('http') && !url.startsWith('#')) {
    url = '/' + url
  }

  return url
})
</script>