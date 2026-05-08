<script setup>
import { ref, computed } from 'vue'

const config = useRuntimeConfig()
const strapiUrl = config.public.strapiUrl

const { data: postsResponse, pending, error } = await useFetch(`${strapiUrl}/api/blog-posts?populate=*`)

const searchQuery = ref('')
const getPostData = (item) => item.attributes || item

const getAuthorName = (post) => {
    const data = getPostData(post)
    const authorData = data.author?.data?.attributes || data.author
    return authorData?.Name || authorData?.name || 'Unknown Author'
}

const filteredPosts = computed(() => {
    const posts = postsResponse.value?.data || []
        
    if (!searchQuery.value) {
        return posts
    }
        
    const query = searchQuery.value.toLowerCase()
    
    return posts.filter(post => {
        const title = (getPostData(post).Title || '').toLowerCase()
        const author = getAuthorName(post).toLowerCase()        
        return title.includes(query) || author.includes(query)
    })
})
</script>

<template>
    <div class="search-container">
        <header class="header">
            <h1>Search the Blog</h1>
            
            <div class="search-bar">
                <input 
                    type="text" 
                    v-model="searchQuery" 
                    placeholder="Search for a post title or author name..." 
                />
            </div>
        </header>

        <div v-if="pending" class="loading-state">
            <p>Loading posts...</p>
        </div>
        
        <div v-else-if="error" class="error-state">
            <p>Error loading data. Please check your connection to Strapi.</p>
        </div>

        <section v-else class="posts-grid">
            <div v-if="filteredPosts.length === 0" class="no-results">
                <p>No results found for "{{ searchQuery }}". Try a different term!</p>
            </div>

            <BlogCard 
                v-for="post in filteredPosts" 
                :key="post.id" 
                :post="post" 
            />
        </section>
    </div>
</template>

<style scoped>
.search-container {
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
  font-family: sans-serif;
}

.header {
  margin-bottom: 2rem;
}

.header h1 {
    margin-bottom: 1rem;
}

.search-bar input {
  width: 100%;
  padding: 1rem;
  font-size: 1.1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-sizing: border-box; /* Ensures padding doesn't break the 100% width */
}

.search-bar input:focus {
    outline: none;
    border-color: #007bff;
    box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.25);
}

.posts-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.5rem;
}

.loading-state, .error-state, .no-results {
  grid-column: 1 / -1;
  text-align: center;
  color: #666;
  padding: 2rem;
}

.error-state {
    color: #dc3545;
}
</style>