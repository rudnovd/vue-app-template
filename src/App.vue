<template>
  <main class="page">
    <section class="links">
      <li v-for="(linksArray, key) in links" :key="key">
        <img :src="getGroupLogo(key)" :alt="key" />
        <ul v-for="link in linksArray" :key="link">
          <a :href="`#${link.toLocaleLowerCase()}`">{{ link }}</a>
        </ul>
      </li>
    </section>

    <section id="pinia">
      <h2>Pinia</h2>

      <pre>
        <code>npm i pinia</code>
      </pre>

      <strong>src/stores/index.ts</strong>
      <pre>
        <code>
import { createPinia } from 'pinia'

const pinia = createPinia()

export default pinia
        </code>
      </pre>

      <strong>src/stores/module.ts</strong>
      <pre>
        <code>
import { defineStore } from 'pinia'

export const useExampleStore = defineStore({
  id: 'example',
  state: () => ({
    count: 0,
  }),
  getters: {
    doubleCount: (state) => state.count * 2,
  },
  actions: {
    async increment() {
      this.count++
    },
  },
})
        </code>
      </pre>

      <strong>src/main.ts</strong>
      <pre>
        <code>
import pinia from '@/stores'
app.use(pinia)
        </code>
      </pre>
    </section>

    <section id="vue-router">
      <h2>Vue-router</h2>
      <pre>
        <code>npm i vue-router</code>
      </pre>

      <strong>src/router.ts</strong>
      <pre>
        <code>
import { createRouter, createWebHistory } from 'vue-router'

const router = createRouter({
  history: createWebHistory('/'),
  routes: [
    {
      path: '/home',
      component: () => import('@/pages/HomePage.vue'),
      meta: {
        title: 'Home',
      },
    },
    {
      path: '/:pathMatch(.*)*',
      name: 'not-found',
      component: () => import('@/pages/NotFoundPage.vue'),
      meta: {
        title: 'Page not found',
      },
    },
  ],
  scrollBehavior(to, from, savedPosition) {
    return new Promise((resolve) => {
      if (savedPosition) return resolve(savedPosition)
      else if (to.hash) return resolve({ el: to.hash })
      else return resolve({ left: 0, top: 0 })
    })
  },
})

router.resolve({
  name: 'not-found',
  params: { pathMatch: ['not', 'found'] },
}).href

router.beforeEach((to, from, next) => {
  if (to.meta.disabled) next({ path: '/' })
  next()
})

router.afterEach((to) => {
  document.title = to.meta.title as string
})

export default router
        </code>
      </pre>

      <strong>src/main.ts</strong>
      <pre>
        <code>
import router from '@/router'

app.use(router)
        </code>
      </pre>
    </section>

    <section id="sass">
      <h2>Sass</h2>

      <pre>npm i -D sass</pre>

      <strong>src/styles/mixins.scss</strong>
      <pre>
        <code>
@mixin media-large {
  @media (min-width: 1280px) {
    @content;
  }
}
        </code>
      </pre>

      <strong>src/styles/variables.scss</strong>
      <pre>
        <code>$color-white: rgb(255, 255, 255);</code>
      </pre>

      <strong>vite.config.ts</strong>
      <pre>
        <code>
css: {
  preprocessorOptions: {
    scss: {
      additionalData: `
        @use "sass:color";
        @import "@/styles/mixins";
      `,
    },
  },
},
        </code>
      </pre>
    </section>

    <section id="stylelint">
      <h2>Stylelint</h2>

      <pre>
        <code>npm i -D stylelint stylelint-config-recommended-vue stylelint-config-recess-order stylelint-config-prettier stylelint-prettier</code>
      </pre>

      <section class="select-scss">
        <strong>{{ scss ? 'with' : 'without' }} SCSS</strong>
        <input id="switch" type="checkbox" :value="scss" :checked="scss" @click="scss = !scss" />
        <label for="switch">With SCSS</label>
      </section>

      <template v-if="scss">
        <pre>
          <code>npm i -D stylelint-config-recommended-scss</code>
        </pre>

        <strong>.stylelintrc</strong>
        <pre>
          <code>
{
  "extends": [
    "stylelint-config-recommended-scss",
    "stylelint-config-recommended-vue/scss",
    "stylelint-config-recess-order",
    "stylelint-config-prettier",
    "stylelint-prettier/recommended"
  ],
  "rules": {
    "property-no-unknown": null
  },
  "overrides": [
    {
      "files": ["*.vue", "**/*.vue", ".scss", "**/*.scss"]
    }
  ]
}
          </code>
        </pre>
      </template>

      <template v-else>
        <strong>.stylelintrc</strong>
        <pre>
          <code>
{
  "extends": [
    "stylelint-config-recommended-vue",
    "stylelint-config-recess-order",
    "stylelint-config-prettier",
    "stylelint-prettier/recommended"
  ],
  "rules": {
    "property-no-unknown": null
  },
  "overrides": [
    {
      "files": ["*.vue", "**/*.vue", ".css", "**/*.css"]
    }
  ]
}
          </code>
        </pre>
      </template>

      <strong>.vscode/settings.json</strong>
      <pre>
        <code>
{
  "stylelint.validate": [
    {{ scss ? '"scss"' : '"css"' }},
    "vue"
  ]
}
        </code>
      </pre>

      <strong>package.json</strong>
      <pre>
        <code>
{
  "scripts": {
    "stylelint": "stylelint '**/*{.scss,.vue}' --fix",
  }
}
        </code>
      </pre>
    </section>

    <section id="commitlint">
      <h2>Commitlint</h2>

      <pre>
        <code>npm i -D @commitlint/cli @commitlint/config-conventional cz-conventional-changelog husky</code>
      </pre>

      <strong>src/commitlint.config.js</strong>
      <pre>
        <code>
module.exports = {
  extends: ['@commitlint/config-conventional']
}
        </code>
      </pre>

      <pre><code>npx husky install && npx husky add .husky/commit-msg 'npx --no -- commitlint --edit $1'</code></pre>

      <strong>package.json</strong>
      <pre>
        <code>
{
  "config": {
    "commitizen": {
      "path": "./node_modules/cz-conventional-changelog"
    }
  }
}
        </code>
      </pre>
    </section>

    <section id="PWA">
      <h2>PWA</h2>

      <pre>
        <code>npm i -D workbox-build workbox-core workbox-precaching workbox-routing workbox-strategies vite-plugin-pwa</code>
      </pre>

      <strong>tsconfig.json</strong>
      <pre>
        <code>
{
  "compilerOptions": {
    "types": ["vite-plugin-pwa/client"],
    "lib": ["WebWorker"],
  }
}
        </code>
      </pre>

      <strong>vite.config.ts</strong>
      <pre>
        <code>
import { VitePWA, VitePWAOptions } from 'vite-plugin-pwa'

const pwaOptions: Partial&lt;VitePWAOptions&gt; = {
  base: '/',
  includeAssets: [
    'favicon.svg',
    'favicon.ico',
    'robots.txt',
    'sitemap.txt',
    'favicon-apple.png',
    'favicon-192.png',
    'favicon-maskable-192.png',
    'favicon-512.png',
  ],
  strategies: 'injectManifest',
  srcDir: 'src',
  filename: 'sw.ts',
  manifest: {
    name: 'App name',
    short_name: 'App short name',
    description: 'App description',
    orientation: 'portrait',
    theme_color: '#000000',
    background_color: '#000000',
    start_url: '/',
    id: '/',
    icons: [
      {
        src: 'favicon-192.png',
        sizes: '192x192',
        type: 'image/png',
      },
      {
        src: 'favicon-maskable-192.png',
        sizes: '192x192',
        type: 'image/png',
        purpose: 'any maskable',
      },
      {
        src: 'favicon-512.png',
        sizes: '512x512',
        type: 'image/png',
      },
    ],
  },
  workbox: {
    sourcemap: true,
  },
}

export default defineConfig({
  plugins: [VitePWA(pwaOptions)]
})
        </code>
      </pre>

      <strong>src/sw.ts</strong>
      <pre>
        <code>
import { cleanupOutdatedCaches, createHandlerBoundToURL, precacheAndRoute } from 'workbox-precaching'

declare let self: ServiceWorkerGlobalScope

self.addEventListener('message', (event) => {
  if (event.data && event.data.type === 'SKIP_WAITING') self.skipWaiting()
})

// self.__WB_MANIFEST is default injection point
precacheAndRoute(self.__WB_MANIFEST)

// clean old assets
cleanupOutdatedCaches()

// to allow work offline
registerRoute(new NavigationRoute(createHandlerBoundToURL('index.html')))
        </code>
      </pre>

      <strong>src/App.vue</strong>
      <pre>
        <code>
import { useRegisterSW } from 'virtual:pwa-register/vue'

const { updateServiceWorker, needRefresh } = useRegisterSW({
  immediate: false,
  onRegistered(registration) {},
  onRegisterError(error) {},
})
        </code>
      </pre>
    </section>
  </main>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const links = {
  ecosystem: ['Pinia', 'Vue-router'],
  styles: ['Sass', 'Stylelint'],
  other: ['Commitlint', 'PWA'],
}

const getGroupLogo = (group: string) => {
  if (group === 'ecosystem') return 'https://api.iconify.design/logos/vue.svg'
  else if (group === 'styles') return 'https://api.iconify.design/logos/sass.svg'
  else return 'https://api.iconify.design/logos/javascript.svg'
}

const scss = ref(true)
</script>

<style>
main {
  padding: 0 16px;
  display: grid;
  gap: 24px;
}

li {
  display: flex;
  gap: 16px;
}

li > img {
  width: 16px;
}

.links {
  display: grid;
  gap: 8px;
}

ul {
  padding: 0;
  margin: 0;
}

pre {
  display: flex;
  align-items: center;
  background: #f8f8f8;
  border-radius: 6px;
  padding: 1rem;
  margin: 1rem 0;
  font-family: ui-monospace, SFMono-Regular, SF Mono, Menlo, Consolas, Liberation Mono, monospace;
  overflow: auto;
  font-size: 85%;
  line-height: 1.45;
}

.select-scss {
  display: flex;
  align-items: flex-end;
  margin-bottom: 16px;
}

.select-scss input[type='checkbox'] {
  appearance: none;
}

.select-scss label {
  cursor: pointer;
  text-indent: -9999px;
  width: 40px;
  height: 21px;
  background: grey;
  display: block;
  border-radius: 100px;
  position: relative;
}

.select-scss label:after {
  content: '';
  position: absolute;
  top: 2px;
  left: 4px;
  width: 18px;
  height: 18px;
  background: #fff;
  border-radius: 90px;
  transition: 0.3s;
}

.select-scss input:checked + label {
  background: rgb(205, 103, 153);
}

.select-scss input:checked + label:after {
  left: calc(100% - 5px);
  transform: translateX(-100%);
}

.select-scss label:active:after {
  width: 16px;
}
</style>
