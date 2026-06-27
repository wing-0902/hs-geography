<script setup lang="ts">
import { joinURL } from 'ufo';

const slugArray = useRoute().params.slug; // [...slug] の場合は配列になる

// 配列を "dir1/dir2/page" のようなスラッシュ区切りの文字列に変換
// 万が一空（ルートパス）の場合を考慮して、空文字をデフォルト値に設定
const slugPath = Array.isArray(slugArray) ? slugArray.join('/') : slugArray;

// useAsyncDataのキーやpathに結合した文字列を渡す
const { data: post } = await useAsyncData(`dict-${slugPath}`, () => {
  return queryCollection('dictionary').path(`/dict/${slugPath}`).first();
});

useSeoMeta({
  title: post.value?.title + ' - 地理探求 - dict.digital'
});

definePageMeta({
  contentMain: true,
  share: true
});

const route = useRoute();
route.meta.menuTitle = post.value?.title || 'Dictionary';
</script>

<template>
  <!-- Render the post as Prose & Vue components -->
  <main data-pagefind-body w-full v-if="post" class="root">
    <h1>
      <span data-pagefind-meta="title">{{ post.title }}</span>
      <small class="detail" v-if="post.category"
        >（
        <span data-pagefind-meta="category">{{ post.category }}</span>
        ）</small
      >
    </h1>
    <article w-full class="content">
      <ContentRenderer :value="post" />
    </article>

    <div data-pagefind-ignore w-full flex justify-center gap-4>
      <NuxtLink
        :href="
          joinURL(
            'https://raw.githubusercontent.com/wing-0902/hs-geography/refs/heads/main/content/',
            post.path,
            'index.md'
          )
        "
        target="_blank"
      >
        <span i-material-symbols-light-docs></span>
        Markdownを表示
      </NuxtLink>
      <NuxtLink
        :to="
          joinURL(
            'https://github.com/wing-0902/hs-geography/edit/main/content',
            post.path,
            'index.md'
          )
        "
        target="_blank"
      >
        <span i-material-symbols-light-edit></span>
        編集を提案
      </NuxtLink>
    </div>
  </main>
  <div v-else>
    <h1>Not Found</h1>
    <p>読み込み中またはコンテンツが存在しません．</p>
  </div>
</template>

<style lang="scss" scoped>
.root {
  min-width: 0;
  overflow-x: hidden;
}

.detail {
  font-family: 'Zen Maru Gothic', sans-serif;
  font-size: 18px;
}

.content {
  padding: 0 4px;
  overflow-x: hidden;
  :deep() {
    width: 100%;
    display: flow-root;
    h2,
    h3,
    h4,
    h5,
    h6 {
      a {
        color: white;
        text-decoration: none;
        &::before {
          content: '#';
          transition: opacity 0.3s ease;
          opacity: 0;
          left: 0;
          color: white;
        }
        &:hover::before {
          opacity: 0.3;
        }
      }
    }
    pre {
      max-width: 100%;
      overflow-x: scroll;
      white-space: pre;
      word-wrap: break-word;
    }
    code {
      word-break: break-all;
    }
    .openStreetMap {
      height: calc(100dvh - 100px);
      max-height: 550px;
      width: 100%;
      border: 1px solid var(--codeBack);
    }
  }
}
</style>
