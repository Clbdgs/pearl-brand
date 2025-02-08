<template>
  <div class="category-page">
    <!-- 分类导航 -->
    <section class="category-nav">
      <el-menu mode="horizontal" :default-active="activeCategory" @select="handleCategorySelect">
        <el-menu-item v-for="category in categories" :key="category.id" :index="category.id">
          {{ category.name }}
        </el-menu-item>
      </el-menu>
    </section>

    <!-- 筛选 & 排序 -->
    <section class="filter-sort">
      <el-row :gutter="20">
        <el-col :span="6">
          <el-select v-model="filters.price" placeholder="价格区间">
            <el-option label="¥0-500" value="0-500"></el-option>
            <el-option label="¥500-1000" value="500-1000"></el-option>
            <el-option label="¥1000以上" value="1000-"></el-option>
          </el-select>
        </el-col>
        <el-col :span="6">
          <el-select v-model="filters.material" placeholder="材质">
            <el-option label="珍珠" value="pearl"></el-option>
            <el-option label="银" value="silver"></el-option>
            <el-option label="黄金" value="gold"></el-option>
          </el-select>
        </el-col>
        <el-col :span="6">
          <el-select v-model="filters.style" placeholder="风格">
            <el-option label="简约" value="simple"></el-option>
            <el-option label="复古" value="vintage"></el-option>
            <el-option label="潮流" value="trendy"></el-option>
          </el-select>
        </el-col>
        <el-col :span="6">
          <el-select v-model="sortBy" placeholder="排序方式">
            <el-option label="默认排序" value="default"></el-option>
            <el-option label="价格从低到高" value="price-asc"></el-option>
            <el-option label="价格从高到低" value="price-desc"></el-option>
            <el-option label="销量最高" value="sales"></el-option>
            <el-option label="最新上架" value="newest"></el-option>
          </el-select>
        </el-col>
      </el-row>
    </section>

    <!-- 商品列表 -->
    <section class="product-list">
      <el-row :gutter="20">
        <el-col v-for="product in products" :key="product.id" :span="6">
          <ProductCard :product="product" @add-to-cart="handleAddToCart" />
        </el-col>
      </el-row>
    </section>

    <!-- 分页 -->
    <section class="pagination">
      <el-pagination
        background
        layout="prev, pager, next"
        :total="totalProducts"
        :page-size="pageSize"
        @current-change="handlePageChange"
      />
    </section>
  </div>
</template>

<script>
import ProductCard from '@/components/ProductCard.vue';

export default {
  components: {
    ProductCard,
  },
  data() {
    return {
      activeCategory: '1',
      categories: [
        { id: '1', name: '项链' },
        { id: '2', name: '手链' },
        { id: '3', name: '戒指' },
        { id: '4', name: '耳饰' },
      ],
      filters: {
        price: '',
        material: '',
        style: '',
      },
      sortBy: 'default',
      products: [],
      totalProducts: 0,
      pageSize: 12,
      currentPage: 1,
    };
  },
  async asyncData({ $axios }) {
    // const { data } = await $axios.get('/api/products');
    // return { products: data.products, totalProducts: data.total };
  const products = [
    { id: 1, name: '简约珍珠项链', price: 299, image: require('@/assets/banner1.png'), description: '经典简约设计，适合日常佩戴', rating: 4.5, reviews: 120 },
    { id: 2, name: '时尚珍珠手链', price: 199, image: require('@/assets/banner2.png'), description: '时尚潮流，彰显个性', rating: 4.2, reviews: 98 },
    { id: 3, name: '优雅珍珠戒指', price: 159, image: require('@/assets/banner1.png'), description: '优雅精致，提升气质', rating: 4.7, reviews: 150 },
    { id: 4, name: '个性珍珠耳饰', price: 179, image: require('@/assets/banner2.png'), description: '独特设计，展现个性魅力', rating: 4.3, reviews: 110 },
    { id: 5, name: '奢华珍珠套装', price: 899, image: require('@/assets/banner1.png'), description: '全套珍珠首饰，彰显高贵', rating: 4.8, reviews: 200 },
    { id: 6, name: '潮流珍珠吊坠', price: 259, image: require('@/assets/banner2.png'), description: '时尚设计，适合各种场合', rating: 4.4, reviews: 130 },
    { id: 7, name: '复古珍珠胸针', price: 199, image: require('@/assets/banner1.png'), description: '复古风格，独特魅力', rating: 4.1, reviews: 90 },
    { id: 8, name: '珍珠发饰套装', price: 159, image: require('@/assets/banner2.png'), description: '多种款式，打造完美造型', rating: 4.6, reviews: 180 }
  ];

  return { 
    products: products.slice(0, 12),
    totalProducts: products.length
  };
  },
  methods: {
    handleCategorySelect(categoryId) {
      this.activeCategory = categoryId;
      this.fetchProducts();
    },
    handlePageChange(page) {
      this.currentPage = page;
      this.fetchProducts();
    },
    handleAddToCart(product) {
      // 处理加入购物车逻辑
      this.$message.success(`${product.name} 已加入购物车`);
    },
    async fetchProducts() {
    //   const { data } = await $axios.get('/api/products', {
    //     params: {
    //       category: this.activeCategory,
    //       ...this.filters,
    //       sort: this.sortBy,
    //       page: this.currentPage,
    //       pageSize: this.pageSize,
    //     },
    //   });
      const data = {
        products: [
          { id: 9, name: '珍珠手镯', price: 399, image: require('@/assets/banner1.png'), description: '简约大气，适合各种场合', rating: 4.6, reviews: 150 },
          { id: 10, name: '珍珠耳钉', price: 129, image: require('@/assets/banner2.png'), description: '小巧精致，日常百搭', rating: 4.3, reviews: 100 },
          { id: 11, name: '珍珠发夹', price: 89, image: require('@/assets/banner1.png'), description: '时尚实用，打造完美发型', rating: 4.2, reviews: 80 },
          { id: 12, name: '珍珠胸针', price: 199, image: require('@/assets/banner2.png'), description: '复古优雅，提升气质', rating: 4.5, reviews: 120 },
          { id: 13, name: '珍珠脚链', price: 159, image: require('@/assets/banner1.png'), description: '独特设计，展现个性', rating: 4.1, reviews: 90 },
          { id: 14, name: '珍珠手链', price: 229, image: require('@/assets/banner2.png'), description: '精致优雅，适合各种场合', rating: 4.4, reviews: 110 },
          { id: 15, name: '珍珠戒指', price: 179, image: require('@/assets/banner1.png'), description: '简约设计，日常佩戴', rating: 4.3, reviews: 100 },
          { id: 16, name: '珍珠项链', price: 299, image: require('@/assets/banner2.png'), description: '经典款式，永不过时', rating: 4.7, reviews: 200 }
        ].slice((this.currentPage - 1) * this.pageSize, this.currentPage * this.pageSize),
        total: 16
      };
      this.products = data.products;
      this.totalProducts = data.total;
    },
  },
};
</script>

<style scoped>
.category-page {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.category-nav {
  margin-bottom: 20px;
}

.filter-sort {
  margin-bottom: 20px;
}

.product-list {
  margin-bottom: 20px;
}

/* 调整商品卡片的上下间距 */
.el-row {
  margin-bottom: 20px;
}

/* 调整商品卡片的左右间距 */
.el-col {
  padding: 10px;
}

/* 确保商品卡片之间有足够的上下间距 */
.product-card {
  margin-bottom: 20px;
}

.pagination {
  text-align: center;
}
</style> 