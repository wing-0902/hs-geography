<script setup lang="ts">
import { ref, onMounted, computed } from 'vue';
import { withQuery } from 'ufo';

const props = withDefaults(
  defineProps<{
    lat: number | string;
    lon: number | string;
    zoom?: number | string;
  }>(),
  {
    zoom: 14
  }
);

const isHidden = ref(true);

onMounted(() => {
  const isAgreed = localStorage.getItem('map_privacy_agreed');
  if (isAgreed === 'true') {
    isHidden.value = false;
  }
});

const handleAccept = () => {
  isHidden.value = false;
  localStorage.setItem('map_privacy_agreed', 'true');
};

const latNum = computed(() => Number(props.lat));
const lonNum = computed(() => Number(props.lon));
const zoomNum = computed(() => Number(props.zoom));

// 緯度・経度・ズームレベルから iframe 用の bbox を計算
const bboxString = computed(() => {
  const degreeDelta = 360 / Math.pow(2, zoomNum.value);
  const aspectRatio = 16 / 9; 
  const lonDelta = degreeDelta * aspectRatio * 0.25;
  const latDelta = degreeDelta * 0.25;

  const minLon = lonNum.value - lonDelta;
  const minLat = latNum.value - latDelta;
  const maxLon = lonNum.value + lonDelta;
  const maxLat = latNum.value + latDelta;

  return `${minLon},${minLat},${maxLon},${maxLat}`;
});

const frameSrc = computed(() => {
  return withQuery('https://www.openstreetmap.org/export/embed.html', {
    bbox: bboxString.value,
    layer: 'mapnik',
    marker: `${latNum.value},${lonNum.value}`
  });
});

const linkHref = computed(() => {
  const fixedLat = latNum.value.toFixed(5);
  const fixedLon = lonNum.value.toFixed(5);
  
  const baseUrlWithQuery = withQuery('https://www.openstreetmap.org/', {
    mlat: fixedLat,
    mlon: fixedLon
  });
  
  return `${baseUrlWithQuery}#map=${zoomNum.value}/${fixedLat}/${fixedLon}`;
});
</script>

<template>
  <div data-pagefind-ignore data-nosnippet flex flex-col w-full px-6 align-right class="mapRootDiv">
    <div
      v-if="isHidden"
      w-full
      h-full
      flex
      flex-col
      items-center
      justify-center
    >
      <p i-material-symbols-light-info text-gray-600 text-2xl></p>
      <p text-xl text-center mb-2 text-gray-600>
        クリックすると外部の地図（OpenStreetMap）を読み込みます．
      </p>
      <p text-sm text-center mb-4 text-gray-600>
        （この機能では，外部のウェブサイトがあなたやデバイスに関する情報を収集する場合があります．）
      </p>
      <button
        mt-4
        border-none
        bg-transparent
        text-xl
        @click="handleAccept"
        class="handleAcceptButton"
      >
        地図を表示する
      </button>
    </div>

    <iframe v-else w-full h-full :src="frameSrc" style="border: none;"></iframe>

    <NuxtLink text-right :to="linkHref" target="_blank" class="mt-2">
      新規タブで開く<span i-material-symbols-light-open-in-new></span>
    </NuxtLink>
  </div>
</template>

<style lang="scss" scoped>
.mapRootDiv {
  height: 400px;
  max-height: calc(100dvh - 150px);
}
.handleAcceptButton {
  color: var(--themeColor);
  font-family: 'Zen Kaku Gothic New', sans-serif;
}
</style>