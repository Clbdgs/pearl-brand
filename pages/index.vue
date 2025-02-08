<template>
  <div class="index-page">
    <!-- 轮播图 -->
    <el-carousel height="500px" indicator-position="outside" arrow="always">
      <el-carousel-item v-for="item in banners" :key="item.id">
        <img :src="item.image" class="banner-img" loading="lazy" />
        <div class="banner-overlay">
          <h2 class="banner-title">{{ item.title }}</h2>
          <p class="banner-description">{{ item.description }}</p>
          <el-button type="primary" round>立即探索</el-button>
        </div>
      </el-carousel-item>
    </el-carousel>

    <!-- 品牌介绍 -->
    <el-container class="brand-intro">
      <el-row justify="center" align="middle">
        <el-col :span="12" class="brand-text">
          <h1 class="title">潮流珍珠 - 个性时尚品牌</h1>
          <p class="subtitle">专为年轻男性、潮流人士打造的个性化珍珠首饰</p>
          <p class="description">
            我们致力于将传统珍珠与现代潮流结合，打造独一无二的时尚单品。无论是日常穿搭还是特殊场合，我们的产品都能让你脱颖而出。
          </p>
          <el-button type="primary" round>了解更多</el-button>
        </el-col>
        <el-col :span="12" class="brand-image">
          <img src="~/assets/banner1.png" alt="品牌形象" class="brand-img" />
        </el-col>
      </el-row>
    </el-container>

    <!-- 热门产品 -->
    <el-container class="featured-products">
      <el-header>
        <h2 class="section-title">热门产品</h2>
        <p class="section-subtitle">精选潮流单品，展现你的独特风格</p>
      </el-header>
      <el-row :gutter="20" justify="center" class="product-grid">
        <el-col v-for="product in products" :key="product.id" :xs="24" :sm="12" :md="8" :lg="6">
          <el-card shadow="hover" class="product-card">
            <img :src="product.image" class="product-img" loading="lazy" />
            <h3>{{ product.name }}</h3>
            <p class="product-description">{{ product.description }}</p>
            <el-button type="primary" round>查看详情</el-button>
          </el-card>
        </el-col>
      </el-row>
    </el-container>

    <!-- 客户评价 -->
    <el-container class="testimonials">
      <el-header>
        <h2 class="section-title">客户评价</h2>
        <p class="section-subtitle">听听他们怎么说</p>
      </el-header>
      <el-row :gutter="20" justify="center">
        <el-col v-for="testimonial in testimonials" :key="testimonial.id" :xs="24" :sm="12" :md="8">
          <el-card shadow="hover" class="testimonial-card">
            <p class="testimonial-text">{{ testimonial.text }}</p>
            <div class="testimonial-author">
              <img :src="testimonial.avatar" alt="客户头像" class="testimonial-avatar" />
              <p class="testimonial-name">{{ testimonial.name }}</p>
            </div>
          </el-card>
        </el-col>
      </el-row>
    </el-container>

    <!-- 底部CTA -->
    <el-container class="cta-section">
      <el-row style="width: 100%;" justify="center" align="middle">
        <el-col :span="24" class="cta-content">
          <h2 class="cta-title">加入潮流，展现自我</h2>
          <p class="cta-description">立即探索我们的产品，开启你的时尚之旅</p>
          <el-button type="primary" round>立即购买</el-button>
        </el-col>
      </el-row>
    </el-container>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';

const banners = ref([
  { id: 1, image: require('@/assets/banner1.png'), title: '潮流珍珠，定义新时尚', description: '探索我们的最新系列' },
  { id: 2, image: require('@/assets/banner2.png'), title: '个性单品，展现独特魅力', description: '打造属于你的风格' }
]);

const products = ref([
  { id: 1, name: '珍珠项链', image: require('@/assets/banner1.png'), description: '珍珠项链，时尚百搭，展现个性魅力' },
  { id: 2, name: '珍珠耳环', image: require('@/assets/banner2.png'), description: '珍珠耳环，优雅精致，彰显品味' }
]);

const testimonials = ref([
  { id: 1, text: '这款珍珠项链真的太棒了，质量非常好，设计也很独特！', name: '李先生', avatar: require('@/assets/banner1.png') },
  { id: 2, text: '珍珠耳环非常精致，佩戴后感觉整个人都更有气质了。', name: '张女士', avatar: require('@/assets/banner1.png') }
]);

const searchQuery = ref('');

const handleSearch = () => {
  // 处理搜索逻辑
};
</script>

<style scoped>
.index-page {
  max-width: 1200px;
  margin: 0 auto;
}

.search-box {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.search-input {
  width: 400px;
  transition: box-shadow 0.3s ease;
}

.search-input:hover {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.search-input:focus-within {
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.2);
}

.search-input .el-input__inner {
  border-radius: 20px;
  padding: 10px 20px;
}

.search-input .el-button {
  border-radius: 0 20px 20px 0;
  background-color: var(--primary-color);
  color: var(--light-color);
}

.search-input .el-button:hover {
  background-color: var(--secondary-color);
}

.banner-img { width: 100%; height: 100%; object-fit: cover; }
.banner-overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: white;
}
.brand-intro { padding: 50px 0; background-color: #f9f9f9; }
.brand-text { padding: 20px; }
.brand-img { width: 100%; height: auto; border-radius: 10px; }
.featured-products { padding: 50px 0; }
.testimonials { padding: 50px 0; background-color: #f9f9f9; }
.testimonial-card { text-align: center; }
.testimonial-avatar { width: 60px; height: 60px; border-radius: 50%; }
.cta-section { padding: 50px 0; background-color: #333; color: white; text-align: center;}
</style>
