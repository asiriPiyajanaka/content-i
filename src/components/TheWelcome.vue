<script setup lang="ts">
import { ref, onMounted } from 'vue'
import DOMPurify from 'dompurify'
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'

// Map of icons for dynamic rendering
const iconMap = {
  DocumentationIcon,
  ToolingIcon,
  EcosystemIcon,
  CommunityIcon,
  SupportIcon,
}

// Type for dynamic content
interface DynamicContentItem {
  heading: string
  icon: keyof typeof iconMap
  content: string // Raw HTML string
}

// Mock API returning structured data
const fetchMockContent = (): Promise<DynamicContentItem[]> => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve([
        {
          heading: 'Ecosystem',
          icon: 'EcosystemIcon',
          content: `
            Get official tools and libraries for your project:
            <a href="https://pinia.vuejs.org/" target="_blank" rel="noopener">Pinia</a>,
            <a href="https://router.vuejs.org/" target="_blank" rel="noopener">Vue Router</a>,
            <a href="https://test-utils.vuejs.org/" target="_blank" rel="noopener">Vue Test Utils</a>, and
            <a href="https://github.com/vuejs/devtools" target="_blank" rel="noopener">Vue Dev Tools</a>.
            If you need more resources, we suggest paying
            <a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">Awesome Vue</a>
            a visit.
          `,
        },
      ])
    }, 2000)
  })
}

// State to hold the fetched and sanitized data
const dynamicContent = ref<DynamicContentItem[]>([])

// Fetch and sanitize content on component mount
onMounted(async () => {
  try {
    const content = await fetchMockContent()
    dynamicContent.value = content.map((item) => ({
      ...item,
      content: DOMPurify.sanitize(item.content), // Sanitize the HTML content to prevent XSS attacks. DOMPurify can configure  to allow specific tags or attributes if needed. See https://github.com/cure53/DOMPurify
    }))
  } catch (error) {
    console.error('Error fetching or sanitizing content:', error)
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

    <!-- Dynamic Content -->
    <div v-for="(item, index) in dynamicContent" :key="index">
      <WelcomeItem>
        <template #icon>
          <!-- Dynamically resolve and render the icon -->
          <component :is="iconMap[item.icon]" />
        </template>
        <template #heading>
          {{ item.heading }}
        </template>
        <!-- Render the sanitized content as raw HTML. This is where lot of limitations live. More sanitization and research on this needs to be done -->
        <div v-html="item.content"></div>
      </WelcomeItem>
    </div>
  </div>
</template>

<style scoped>
h1 {
  font-size: 2rem;
  margin-bottom: 1rem;
}
</style>
