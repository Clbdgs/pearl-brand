<template>
  <div class="layout">
    <Header />
    <!-- 页面内容 -->
    <div class="page-content">
      <nuxt />
    </div>
    <Footer />
  </div>
</template>

<script>
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'
export default {
  components: {
    Header,
    Footer
  }
}
</script>

<style>
/* 添加全局样式 */
.layout {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: var(--primary-color);
}

.page-content {
  flex: 1;
  padding-top: 80px; /* 根据头部高度调整 */
  padding-bottom: 100px; /* 根据footer高度调整 */
}
</style>