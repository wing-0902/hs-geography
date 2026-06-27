<script setup lang='ts'>
defineProps({
  frameSrc: String,
  linkHref: String
})
</script>

<template>
  <div flex flex-col w-full px-6 align-right class='mapRootDiv'>
    <iframe w-full h-full :src="frameSrc"></iframe>
    <NuxtLink text-right :to="linkHref" target="_blank">地図を開く<span i-material-symbols-light-open-in-new></span></NuxtLink>
  </div>
</template>

<style lang="scss" scoped>
.mapRootDiv {
  height: 400px;
  max-height: calc(100dvh - 150px);
}
</style>