<script setup>
import { computed } from 'vue'
import { useRoute } from 'nuxt/app'
import { marked } from 'marked'

const route = useRoute()
const config = useRuntimeConfig()
const strapiUrl = config.public.strapiUrl

const pageIdentifier = route.params.identifier

const { data, pending, error } = await useFetch(`${strapiUrl}/api/blog-posts?filters[Identifier][$eq]=${pageIdentifier}&populate=*`)

const postRaw = computed(() => {
    const postsArray = data.value?.data || []
    return postsArray[0] || null
})

const getPostData = (item) => item?.attributes || item

const getAuthorName = (postItem) => {
    if (!postItem) return 'Unknown Author'
    const itemData = getPostData(postItem)
    const authorData = itemData.author?.data?.attributes || itemData.author
    return authorData?.Name || authorData?.name || 'Unknown Author'
}

const parsedContent = computed(() => {
    if (!postRaw.value) return ''
    const rawMarkdown = getPostData(postRaw.value).Content || ''
    return marked(rawMarkdown) // Converts the Markdown string into HTML tags
})
</script>

<template>
    <main class="single-post-container">
        <div v-if="pending" class="message">Loading post...</div>
        <div v-else-if="error" class="message error">Error loading post.</div>
        <div v-else-if="!postRaw" class="message">Post not found.</div>

        <article v-else class="post-content">
            <header class="post-header">
                <NuxtLink to="/" class="back-link">← Back to Blog</NuxtLink>
                <h1>{{ getPostData(postRaw).Title }}</h1>
                <p class="author">Written by: {{ getAuthorName(postRaw) }}</p>
            </header>

            <div class="markdown-body" v-html="parsedContent"></div>
        </article>
    </main>
</template>

<style scoped>
.single-post-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 2rem;
    font-family: sans-serif;
}

.message {
    text-align: center;
    padding: 3rem;
    color: #666;
    font-size: 1.2rem;
}

.error {
    color: #dc3545;
}

.post-header {
    margin-bottom: 2rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid #eaeaea;
}

.back-link {
    display: inline-block;
    margin-bottom: 1rem;
    color: #007bff;
    text-decoration: none;
    font-size: 0.9rem;
}

.back-link:hover {
    text-decoration: underline;
}

.post-header h1 {
    font-size: 2.5rem;
    margin: 0 0 0.5rem 0;
    color: #333;
}

.author {
    color: #666;
    font-style: italic;
    margin: 0;
}

.markdown-body :deep(h2) {
    margin-top: 2rem;
    color: #222;
}

.markdown-body :deep(h3) {
    margin-top: 1.5rem;
    color: #444;
}

.markdown-body :deep(p) {
    line-height: 1.6;
    color: #333;
    margin-bottom: 1.2rem;
}

.markdown-body :deep(code) {
    background-color: #f4f4f4;
    padding: 0.2rem 0.4rem;
    border-radius: 4px;
    font-family: monospace;
}
</style>