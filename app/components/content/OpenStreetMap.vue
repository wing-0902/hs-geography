<script setup lang="ts">
import { ref, onMounted } from 'vue';

defineProps({
  frameSrc: String,
  linkHref: String
});

// 初期値は一端 false にしておく
const isHidden = ref(true);

// クライアントサイドでのマウント時に localStorage を確認
onMounted(() => {
  const isAgreed = localStorage.getItem('map_privacy_agreed');
  if (isAgreed === 'true') {
    isHidden.value = false;
  }
});

// ユーザーが同意してボタンを押した時の処理
const handleAccept = () => {
  isHidden.value = false;
  localStorage.setItem('map_privacy_agreed', 'true');
};
</script>

<template>
  <div flex flex-col w-full px-6 align-right class="mapRootDiv">
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
      <p text-sm text-center mb-4 text-gray-600>
        クリックすると外部の地図（OpenStreetMap）を読み込みます．<br />
        （この機能を利用すると，外部のウェブサイトがあなたやデバイスに関する情報を収集する場合があります．）
      </p>
      <button @click="handleAccept" class="handleAcceptButton">
        地図を表示する
      </button>
    </div>

    <iframe v-else w-full h-full :src="frameSrc"></iframe>

    <NuxtLink text-right :to="linkHref" target="_blank" class="mt-2">
      別タブで地図を開く<span i-material-symbols-light-open-in-new></span>
    </NuxtLink>
  </div>
</template>

<style lang="scss" scoped>
.mapRootDiv {
  height: 400px;
  max-height: calc(100dvh - 150px);
}
.handleAcceptButton {
}
</style>
