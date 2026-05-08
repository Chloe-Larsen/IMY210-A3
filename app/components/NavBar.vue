<script setup>
</script>

<template>
    <nav class="navbar">
        <div class="nav-brand">
            <NuxtLink to="/">MyDevBlog</NuxtLink>
        </div>
    
        <ul class="nav-links">
            <li><NuxtLink to="/">Home</NuxtLink></li>
            <li><NuxtLink to="/search">Search</NuxtLink></li>
        </ul>
    </nav>
</template>

<style scoped>
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background-color: #f8f9fa;
  border-bottom: 1px solid #eaeaea;
  margin-bottom: 2rem;
}

.nav-brand a {
  font-size: 1.5rem;
  font-weight: bold;
  text-decoration: none;
  color: #333;
}

.nav-links {
  list-style: none;
  display: flex;
  gap: 1.5rem;
  margin: 0;
  padding: 0;
}

.nav-links a {
  text-decoration: none;
  color: #007bff;
  font-size: 1.1rem;
}

.nav-links a:hover {
  text-decoration: underline;
}

.nav-links a.router-link-active {
  color: #004494;
  font-weight: bold;
}
</style>