<script setup lang="ts">
import { joinURL } from 'ufo';

const slug = useRoute().params.slug;
const { data: post } = await useAsyncData(`dict-${slug}`, () => {
  return queryCollection('dictionary').path(`/dict/${slug}`).first();
});

useSeoMeta({
  title: post.value?.title + ' - 地理探求'
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
  <div data-pagefind-body w-full v-if="post" class="root">
    <h1>
      <span data-pagefind-meta="title">{{ post.title }}</span>
      <small class="detail" v-if="post.category"
        >（
        <span data-pagefind-meta="category">{{ post.category }}</span>
        ）</small
      >
    </h1>
    <div w-full class="content">
      <ContentRenderer :value="post" />
    </div>

    <div w-full flex justify-center gap-4>
      <a
        :href="
          joinURL(
            'https://github.com/wing-0902/hs-geography/blob/main/content',
            post.path,
            'index.md'
          )
        "
        target="_blank"
      >
        <span i-material-symbols-light-docs></span>
        Markdownを表示
      </a>
      <a
        :href="
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
      </a>
    </div>
  </div>
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
    width: 100%;
    display: flow-root;
  }
}
</style>
