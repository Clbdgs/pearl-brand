<template>
  <div class="blog-page">
    <!-- 搜索 & 筛选 -->
    <div class="filters">
      <div class="search-box">
        <el-input
          v-model="searchQuery"
          placeholder="请输入关键词"
          @keyup.enter="handleSearch"
          class="search-input"
        >
          <el-button slot="append" icon="el-icon-search" @click="handleSearch" />
        </el-input>
      </div>
      <el-select v-model="selectedCategory" placeholder="选择分类" @change="handleCategoryChange">
        <el-option v-for="category in categories" :key="category" :label="category" :value="category" />
      </el-select>
      <el-select v-model="sortBy" placeholder="排序方式" @change="handleSortChange">
        <el-option label="最新" value="latest" />
        <el-option label="最热" value="popular" />
      </el-select>
    </div>

    <!-- 资讯列表 -->
    <div class="blog-list">
      <BlogCard v-for="blog in blogs" :key="blog.id" :blog="blog" />
    </div>
  </div>
</template>

<script>
import BlogCard from '@/components/BlogCard.vue';

export default {
  components: {
    BlogCard,
  },
  data() {
    return {
      searchQuery: '',
      selectedCategory: '',
      sortBy: 'latest',
      categories: ['珍珠搭配', '品牌动态', '潮流趋势'],
      blogs: [],
    };
  },
  async asyncData({ $axios }) {
    return {
      blogs: [
        {
          id: '1',
          title: '珍珠搭配指南：打造时尚造型',
          date: '2023-10-01',
          author: '时尚达人',
          views: 1234,
          likes: 56,
          content: '分享珍珠首饰的搭配技巧...',
          category: '珍珠搭配',
          image: require('@/assets/banner1.png')
        },
        {
          id: '2',
          title: '2023秋季珍珠潮流趋势',
          date: '2023-09-25',
          author: '潮流观察员',
          views: 987,
          likes: 34,
          content: '解析本季最in的珍珠设计...',
          category: '潮流趋势',
          image: require('@/assets/banner1.png')
        },
        {
          id: '3',
          title: '品牌新品发布会回顾',
          date: '2023-09-20',
          author: '品牌小编',
          views: 1567,
          likes: 78,
          content: '带你回顾最新产品发布盛况...',
          category: '品牌动态',
          image: require('@/assets/banner1.png')
        }
      ]
    };
  },
  methods: {
    handleSearch() {
      // 处理搜索逻辑
    },
    handleCategoryChange() {
      // 处理分类筛选逻辑
    },
    handleSortChange() {
      // 处理排序逻辑
    },
  },
};
</script>

<style scoped>
.blog-page {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.filters {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
}

.blog-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}

.search-box {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.search-input:hover {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1); /* 添加阴影效果 */
}

.search-input:focus-within {
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.2); /* 聚焦时加深阴影 */
}

.search-input .el-input__inner {
  border-radius: 20px; /* 添加圆角 */
  padding: 10px 20px; /* 调整内边距 */
}

.search-input .el-button {
  border-radius: 0 20px 20px 0; /* 按钮圆角 */
  background-color: var(--primary-color);
  color: var(--light-color);
}

.search-input .el-button:hover {
  background-color: var(--secondary-color);
}

</style> 