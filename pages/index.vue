<script setup>
    // import { useRuntimeConfig } from 'nuxt/app';
    import {ref, computed} from 'vue'
    const config = useRuntimeConfig();
    const strapiUrl = config.public.strapiUrl;

    const { data: postsResponse } = await useFetch(`${strapiUrl}/api/blog-posts?populate=*`, {
        method: 'POST'
    })
    const { data: categoriesResponse } = await useFetch(`${strapiUrl}/api/categories`)
    const selectedCategory = ref('')

    const filteredPosts = computed(() => {
        const posts = postsResponse.value?.data || []
  
        if (!selectedCategory.value) {
            return posts
        }
  
        return posts.filter(post => {
            return getCategoryNameForPost(post) === selectedCategory.value
        })
    })

    const categories = computed(() => categoriesResponse.value?.data || [])

    const getPostData = (item) => item.attributes || item

    const getAuthorName = (post) => {
        const data = getPostData(post)  
        const authorData = data.Author?.data?.attributes || data.Author
        return authorData?.Name || 'Unknown Author'
    }

    const getCategoryName = (category) => {
        return category.attributes?.Name || category.Name;
    }

    const getCategoryNameForPost = (post) => {
        const data = getPostData(post)
        const categoryData = data.Category?.data?.attributes || data.Category
        return categoryData?.Name || 'Uncategorized'
    }
</script>

<template>
    <div class="main">
        <header>
            <h1>Blog</h1>
            <div class="filter-selection">
                <label for="category-select">Filter by Category:</label>
                <select id="category-select" v-model="selectedCategory">
                    <option value="">All Categories</option>
                    <option 
                        v-for="category in categories" 
                        :key="category.id" 
                        :value="getCategoryName(category)">
                            {{ getCategoryName(category) }}
                    </option>
                </select>
            </div>
        </header>

        <section class="posts-grid">
            <div v-if="filteredPosts.length === 0" class="no-posts">
                <p>No blog posts found for this category.</p>
            </div>

            <article 
                v-for="post in filteredPosts" 
                :key="post.id" 
                class="blog-card">
                    <h2>{{ getPostData(post).Title }}</h2>
                    <p class="author">By: {{ getAuthorName(post) }}</p>
                    <p class="snippet">{{ getPostData(post).Snippet }}</p>
                    <NuxtLink :to="`/${getPostData(post).Identifier}`" class="read-more">Read More</NuxtLink>
            </article>
        </section>
    </div>
</template>

<style scoped>
.main {
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
  font-family: sans-serif;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid #eaeaea;
}

.filter-selection select {
  margin-left: 0.5rem;
  padding: 0.5rem;
  border-radius: 4px;
  border: 1px solid #ccc;
}

.posts-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.5rem;
}

.blog-card {
  border: 1px solid #eaeaea;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  display: flex;
  flex-direction: column;
}

.blog-card h2 {
  margin-top: 0;
  font-size: 1.25rem;
}

.author {
  font-size: 0.875rem;
  color: #666;
  font-style: italic;
  margin-bottom: 1rem;
}

.snippet {
  color: #333;
  flex-grow: 1;
}

.read-more {
  display: inline-block;
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  background-color: #007bff;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  text-align: center;
}

.read-more:hover {
  background-color: #0056b3;
}

.no-posts {
  grid-column: 1 / -1;
  text-align: center;
  color: #666;
  padding: 2rem;
}
</style>