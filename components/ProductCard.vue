<template>
  <div class="product-card">
    <img :src="product.image" :alt="product.name" class="product-image" />
    <div class="product-info">
      <h3>{{ product.name }}</h3>
      <p class="price">¥{{ product.price }}</p>
      <p v-if="product.discount" class="discount">{{ product.discount }}</p>
      <div class="rating">
        <el-rate v-model="product.rating" disabled />
        <span class="review-count">({{ product.reviews }})</span>
      </div>
      <nuxt-link :to="`/product/${product.id}`">
        <el-button type="primary">查看详情</el-button>
      </nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    product: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
.product-card {
  border: 1px solid #eee;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.product-card:hover {
  transform: scale(1.05);
}

.product-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.product-info {
  padding: 20px;
}

.price {
  font-size: 18px;
  font-weight: bold;
  color: #e4393c;
}

.discount {
  color: #999;
  font-size: 14px;
}

.rating {
  display: flex;
  align-items: center;
  margin: 10px 0;
}

.review-count {
  margin-left: 10px;
  color: #999;
}
</style> 