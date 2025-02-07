<template>
  <nuxt-link :to="`/blog/${blog.id}`" class="blog-card">
    <img :src="blog.image" :alt="blog.title" class="cover" />
    <div class="content">
      <h3>{{ blog.title }}</h3>
      <p class="summary">{{ blog.content.substring(0, 100) }}...</p>
      <div class="meta">
        <span>{{ blog.date }}</span>
        <span>阅读量: {{ blog.views }}</span>
        <span>点赞数: {{ blog.likes }}</span>
      </div>
    </div>
  </nuxt-link>
</template>

<script>
export default {
  props: {
    blog: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
.blog-card {
  display: block;
  border: 1px solid #eee;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.blog-card:hover {
  transform: scale(1.05);
}

.cover {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.content {
  padding: 20px;
}

.summary {
  color: #666;
  margin: 10px 0;
}

.meta {
  display: flex;
  justify-content: space-between;
  color: #999;
  font-size: 14px;
}
</style> 