<script setup lang="ts">
import { ref, onMounted } from 'vue'
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
// import EcosystemIcon from './icons/IconEcosystem.vue';
// import CommunityIcon from './icons/IconCommunity.vue';
// import SupportIcon from './icons/IconSupport.vue';

// Mock API to fetch HTML content
const fetchMockContent = (): Promise<string> => {
  return new Promise((resolve) => {
    console.log('Fetching mock content...')
    setTimeout(() => {
      resolve(`
        <WelcomeItem>
          <template #icon>
            <EcosystemIcon />
          </template>
          <template #heading>Ecosystem</template>
          Get official tools and libraries for your project:
          <a href="https://pinia.vuejs.org/" target="_blank" rel="noopener">Pinia</a>,
          <a href="https://router.vuejs.org/" target="_blank" rel="noopener">Vue Router</a>,
          <a href="https://test-utils.vuejs.org/" target="_blank" rel="noopener">Vue Test Utils</a>, and
          <a href="https://github.com/vuejs/devtools" target="_blank" rel="noopener">Vue Dev Tools</a>.
          If you need more resources, we suggest paying
          <a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">Awesome Vue</a>
          a visit.
        </WelcomeItem>
      `)
    }, 1000) // Simulate network delay
  })
}

// State to hold the fetched HTML content
const fetchedContent = ref<string>('')

// Fetch content on component mount
onMounted(async () => {
  try {
    const content = await fetchMockContent()
    fetchedContent.value = content
  } catch (error) {
    console.error('Error fetching mock content:', error)
  }
})
</script>

<template>
  <div>
    <!-- Static Content -->
    <WelcomeItem>
      <template #icon>
        <DocumentationIcon />
      </template>
      <template #heading>Documentation</template>
      Vue’s
      <a href="https://vuejs.org/" target="_blank" rel="noopener">official documentation</a>
      provides you with all information you need to get started.
    </WelcomeItem>

    <WelcomeItem>
      <template #icon>
        <ToolingIcon />
      </template>
      <template #heading>Tooling</template>
      This project is served and bundled with
      <a href="https://vite.dev/guide/features.html" target="_blank" rel="noopener">Vite</a>. The
      recommended IDE setup is
      <a href="https://code.visualstudio.com/" target="_blank" rel="noopener">VSCode</a>
      +
      <a href="https://github.com/johnsoncodehk/volar" target="_blank" rel="noopener">Volar</a>.
    </WelcomeItem>

    <!-- Injected Content -->
    <div v-html="fetchedContent"></div>
  </div>
</template>

<style scoped>
h1 {
  font-size: 2rem;
  margin-bottom: 1rem;
}
</style>
