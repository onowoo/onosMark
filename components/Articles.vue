<template>
    <ContentList :path="path">
      <template #default="{ list }">
        <div class="grid grid-cols-2 mx-6 lg:mx-auto max-w-4xl gap-6 md:grid-cols-4 mt-20 lg:mt-25">
        <div
            v-for="article in list"
            :key="article.body.id"
            v-show="article._path !== '/link'"
            class="documentation-container relative order-last col-span-2 row-span-2 max-w-110 items-center rounded-xl text-black lg:order-none lg:col-span-2 dark:border-transparent hover:border-transparent dark:text-white"
          >
            <div
              class="gradient-border gradient-border-square gradient-border-documentation h-40"
            />
            <div
              flex="~ col gap-2"
              class="h-40 p-3 rounded-xl bg-white lg:flex-col dark:(bg-gray-900/60 hover:bg-[#0c0f27] border-gray-800) border border-gray-200 hover:border-transparent"
            >
              <NuxtLink :to="article._path">
                <div class="line-clamp-1 leading-7">{{ article.title }}</div>
              </NuxtLink>
              <div text="xs gray-400" line-clamp-3 leading-6 flex="1">{{ article.description }}</div>
              <div flex="~ justify-between items-center" text="xs gray-500">
                <div flex="~ items-center gap-2">
                  <div>{{ article.category }}</div>
                  <div>{{ article.date }}</div>
                </div>
                <div flex="~ items-center gap-2">
                <NuxtLink v-for="item in article.tags" :key="item" :to="`/tags/${item}`">
                  <el-tag 
                    size="small"
                    type="info"
                    effect="plain"
                    @click="$router.push(`/tags`)"
                    >{{ item }}
                  </el-tag>
                </NuxtLink>
                </div>
              </div>
            </div>
          </div>
            </div>
      </template>
      <template #not-found>
        <section class="min-h-[80vh] flex items-center h-full p-16 dark:text-gray-100">
            <div class="container flex flex-col items-center justify-center px-5 mx-auto my-8">
              <div class="max-w-md text-center">
                <h2 class="mb-8 font-extrabold text-9xl dark:text-gray-600">
                  <span class="sr-only">Error</span>404
                </h2>
                <p class="text-2xl font-semibold md:text-3xl">对不起 没有此页面</p>
                <p class="mt-4 mb-8 dark:text-gray-400">可以通过顶部菜单浏览更多内容</p>
                <a 
                rel="noopener noreferrer" 
                href="/" 
                class="px-8 py-3 font-semibold rounded bg-[#00dc82] text-gray-900 shadow-lg hover:(bg-#00dc82/80)">
                返回首页
                </a>
              </div>
            </div>
          </section>
      </template>
    </ContentList>

</template>
<script setup>
import { indexMeta,frontendMeta,backendMeta } from '../constants/index'
const { path } = useRoute()
const meta = ref({})
const getMeta = ()=>{
  if (path === '/') {
    meta.value = indexMeta
  } else if (path === '/frontend') {
    meta.value = frontendMeta
  } else {
    meta.value = backendMeta
  }
}
onMounted(()=>{
  getMeta()
  useSeoMeta(meta.value)
})
</script>

<style scoped>
  .gradient-border-documentation {
    background: linear-gradient(
      var(--gradient-angle),
      rgba(0, 220, 130),
      rgba(0, 220, 130, 0.6),
      rgba(255, 255, 255, 0.8),
      rgba(0, 220, 130)
    );
  }

/* gradient border */
.gradient-border {
  opacity: 0;
  position: absolute;
  top: 0;
  left: 0;
  width: calc(100% + 2px);
  border-radius: 12px;
  z-index: -1;
  transform: translate(-1px, -1px);
}

.gradient-border-rect {
  height: calc(100% + 2px);
}

@media (min-width: 1024px) {
  .gradient-border-rect {
    height: calc(100% + 1px);
  }
}

.gradient-border-square {
  height: calc(100% + 2px);
}

.modules-gradient-right {
  opacity: 0;
}

.documentation-container:hover > .gradient-border {
  opacity: 1;
  animation: gradient-rotate 4s cubic-bezier(0, 0, 1, 1) 0s infinite reverse;
  transition: all 0.3s linear;
}

@media (prefers-color-scheme: dark) {
  /* documentation */
  .documentation-image-dark {
    display: block;
  }
  .documentation-image-light {
    display: none;
  }
  .documentation-container:hover > a > div > .documentation-image-color-dark {
    display: block;
  }
  .documentation-container:hover > a > div > .documentation-image-dark {
    display: none;
  }
}

@property --gradient-angle {
  syntax: "<angle>";
  inherits: false;
  initial-value: 180deg;
}

@keyframes gradient-rotate {
  0% {
    --gradient-angle: 0deg;
  }

  100% {
    --gradient-angle: 360deg;
  }
}
</style>
