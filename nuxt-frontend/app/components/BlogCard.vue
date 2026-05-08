<script setup>
const props = defineProps({
    post: {
        type: Object,
        required: true
    }
})

const getPostData = (item) => item.attributes || item

const getAuthorName = (postItem) => {
    const data = getPostData(postItem)  
    const authorData = data.author?.data?.attributes || data.author
    return authorData?.Name || authorData?.name || 'Unknown Author'
}
</script>

<template>
    <div class="blog-card">
        <h2>{{ getPostData(props.post).Title }}</h2>
        <p class="author">By: {{ getAuthorName(props.post) }}</p>
        <p class="snippet">{{ getPostData(props.post).Snippet }}</p>
        <NuxtLink :to="`/${getPostData(props.post).Identifier}`" class="read-more">Read More</NuxtLink>
    </div>
</template>

<style scoped>
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
</style>