<template>
    <div class="product-page">
      <!-- 商品基本信息 -->
      <section class="product-info">
        <div class="product-gallery">
          <el-carousel ref="carousel" :interval="5000" :autoplay="false" arrow="always" @change="resetVideo">
            <!-- 视频播放项 -->
            <el-carousel-item v-if="product.video">
              <div class="video-container">
                <video ref="productVideo" class="product-video" :poster="product.video.poster">
                  <source :src="product.video.url" type="video/mp4">
                  您的浏览器不支持视频播放
                </video>
                <div v-if="!isVideoPlaying" class="video-overlay" @click="playVideo">
                  <i class="el-icon-video-play video-play-button"></i>
                </div>
              </div>
            </el-carousel-item>
            <!-- 图片项 -->
            <el-carousel-item v-for="(image, index) in product.images" :key="index">
              <img :src="image" :alt="product.name" class="product-image" />
            </el-carousel-item>
          </el-carousel>
        </div>
        <div class="product-details">
          <h1>{{ product.name }}</h1>
          <div class="price-section">
            <span class="price">¥{{ calculatedPrice }}</span>
            <span v-if="product.discount" class="discount">¥{{ product.discount }}</span>
          </div>

          <div class="stock-status">
            <el-tag :type="product.stock > 0 ? 'success' : 'danger'">
              {{ product.stock > 0 ? '有货' : '售罄' }}
            </el-tag>
          </div>
          <!-- 规格参数 -->
          <div class="product-specs">
            <div class="specs-container">
              <div class="specs-group">
                <h3>材质</h3>
                <div class="specs-options">
                  <el-button
                    v-for="material in product.materials"
                    :key="material.value"
                    :type="selectedMaterial === material.value ? 'primary' : 'default'"
                    @click="selectMaterial(material.value)"
                  >
                    {{ material.label }}
                  </el-button>
                </div>
              </div>
              <div class="specs-group">
                <h3>尺寸</h3>
                <div class="specs-options">
                  <el-button
                    v-for="size in product.sizes"
                    :key="size.value"
                    :type="selectedSize === size.value ? 'primary' : 'default'"
                    @click="selectSize(size.value)"
                  >
                    {{ size.label }}
                  </el-button>
                </div>
              </div>
              <div class="specs-group">
                <h3>颜色</h3>
                <div class="specs-options">
                  <el-button
                    v-for="color in product.colors"
                    :key="color.value"
                    :type="selectedColor === color.value ? 'primary' : 'default'"
                    @click="selectColor(color.value)"
                  >
                    {{ color.label }}
                  </el-button>
                </div>
              </div>
            </div>
          </div>
          <div class="actions">
            <el-button type="primary" @click="showWechatQRCode">立即购买</el-button>
            <el-button @click="handleAddToCart">加入购物车</el-button>
            <el-button type="text" @click="handleAddToWishlist">收藏</el-button>
          </div>
          <div class="social-share">
            <el-button type="text" @click="shareToWechat">微信</el-button>
            <el-button type="text" @click="shareToWeibo">微博</el-button>
            <el-button type="text" @click="shareToXiaohongshu">小红书</el-button>
          </div>
        </div>
      </section>
  
      <!-- 微信小程序二维码弹框 -->
      <el-dialog :visible.sync="showQRCode" width="30%">
        <div class="qr-code-container">
          <img src="@/assets/wechat-qr.png" alt="微信小程序二维码" class="qr-code" />
          <p>扫描二维码，立即购买</p>
        </div>
      </el-dialog>
  
      <!-- 商品描述 & 详情 -->
      <section class="product-description">
        <h2>商品描述</h2>
        <div v-html="product.description"></div>
        <div class="product-highlights">
          <h3>产品亮点</h3>
          <ul>
            <li v-for="highlight in product.highlights" :key="highlight">{{ highlight }}</li>
          </ul>
        </div>
        <div class="product-details">
          <h3>材质解析</h3>
          <p>{{ product.materialDetails }}</p>
        </div>
        <div class="product-images">
          <img v-for="image in product.detailImages" :key="image" :src="image" :alt="product.name" />
        </div>
      </section>
  
      <!-- 用户评价 & 晒单 -->
      <section class="product-reviews">
        <h2>用户评价</h2>
        <div class="rating-section">
          <el-rate v-model="product.rating" disabled />
          <span class="review-count">{{ product.reviews.length }} 条评价</span>
        </div>
        <div class="reviews">
          <div v-for="review in product.reviews" :key="review.id" class="review">
            <div class="review-header">
              <el-avatar :src="review.avatar" />
              <span class="review-author">{{ review.author }}</span>
              <el-rate v-model="review.rating" disabled />
            </div>
            <p class="review-text">{{ review.text }}</p>
            <div v-if="review.images" class="review-images">
              <img v-for="image in review.images" :key="image" :src="image" />
            </div>
          </div>
        </div>
      </section>
  
      <!-- 相关推荐 -->
      <section class="related-products">
        <h2>相关推荐</h2>
        <div class="product-grid">
          <el-card v-for="relatedProduct in relatedProducts" :key="relatedProduct.id" class="product-card">
            <img :src="relatedProduct.image" :alt="relatedProduct.name" class="product-image" />
            <h3>{{ relatedProduct.name }}</h3>
            <p class="product-price">¥{{ relatedProduct.price }}</p>
            <el-button type="primary" @click="viewProduct(relatedProduct.id)">查看详情</el-button>
          </el-card>
        </div>
      </section>
    </div>
  </template>
  
  <script>
  export default {
    async asyncData({ params, $axios }) {
    //   const { data: product } = await $axios.get(`/api/product/${params.id}`);
    //   const { data: relatedProducts } = await $axios.get('/api/related-products', { params: { productId: params.id } });
    //   return { product, relatedProducts };
    const product = {
      id: params.id,
      name: '简约珍珠项链',
      price: 299,
      discount: 399,
      stock: 1,
      images: [
        require('@/assets/banner1.png'),
        require('@/assets/banner2.png')
      ],
      video: {
        url: 'https://www.example.com/video.mp4',
        poster: require('@/assets/banner1.png')
      },
      materials: [
        { value: 'pearl', label: '珍珠' },
        { value: 'gold', label: '黄金' },
        { value: 'silver', label: '银' }
      ],
      sizes: [
        { value: 'small', label: '小号' },
        { value: 'medium', label: '中号' },
        { value: 'large', label: '大号' }
      ],
      colors: [
        { value: 'white', label: '白色' },
        { value: 'black', label: '黑色' },
        { value: 'gold', label: '金色' }
      ],
      specs: [
        { material: 'pearl', size: 'small', color: 'white', price: 299 },
        { material: 'pearl', size: 'small', color: 'black', price: 309 },
        { material: 'pearl', size: 'small', color: 'gold', price: 319 },
        { material: 'pearl', size: 'medium', color: 'white', price: 319 },
        { material: 'pearl', size: 'medium', color: 'black', price: 329 },
        { material: 'pearl', size: 'medium', color: 'gold', price: 339 },
        { material: 'pearl', size: 'large', color: 'white', price: 339 },
        { material: 'pearl', size: 'large', color: 'black', price: 349 },
        { material: 'pearl', size: 'large', color: 'gold', price: 359 },
        { material: 'gold', size: 'small', color: 'white', price: 399 },
        { material: 'gold', size: 'small', color: 'black', price: 409 },
        { material: 'gold', size: 'small', color: 'gold', price: 419 },
        { material: 'gold', size: 'medium', color: 'white', price: 419 },
        { material: 'gold', size: 'medium', color: 'black', price: 429 },
        { material: 'gold', size: 'medium', color: 'gold', price: 439 },
        { material: 'gold', size: 'large', color: 'white', price: 439 },
        { material: 'gold', size: 'large', color: 'black', price: 449 },
        { material: 'gold', size: 'large', color: 'gold', price: 459 },
        { material: 'silver', size: 'small', color: 'white', price: 349 },
        { material: 'silver', size: 'small', color: 'black', price: 359 },
        { material: 'silver', size: 'small', color: 'gold', price: 369 },
        { material: 'silver', size: 'medium', color: 'white', price: 369 },
        { material: 'silver', size: 'medium', color: 'black', price: 379 },
        { material: 'silver', size: 'medium', color: 'gold', price: 389 },
        { material: 'silver', size: 'large', color: 'white', price: 389 },
        { material: 'silver', size: 'large', color: 'black', price: 399 },
        { material: 'silver', size: 'large', color: 'gold', price: 409 },
      ],
      description: '经典简约设计，适合日常佩戴',
      rating: 4.5,
      reviews: [
        {
          id: 1,
          author: '用户A',
          avatar: require('@/assets/banner1.png'),
          rating: 5,
          text: '非常喜欢，质量很好',
          images: [require('@/assets/banner1.png')]
        },
        {
          id: 2,
          author: '用户B',
          avatar: require('@/assets/banner1.png'),
          rating: 4,
          text: '款式不错，性价比高',
          images: []
        }
      ],
      materialDetails: '采用优质淡水珍珠，搭配925纯银扣头',
      detailImages: [
        require('@/assets/banner1.png'),
        require('@/assets/banner2.png')
      ]
    };

    const relatedProducts = [
      {
        id: 2,
        name: '时尚珍珠手链',
        price: 199,
        image: require('@/assets/banner2.png')
      },
      {
        id: 3,
        name: '优雅珍珠戒指',
        price: 159,
        image: require('@/assets/banner1.png')
      },
      {
        id: 4,
        name: '个性珍珠耳饰',
        price: 179,
        image: require('@/assets/banner2.png')
      }
    ];

    return { product, relatedProducts };
    },
    data() {
      return {
        isVideoPlaying: false,
        showQRCode: false,
        selectedMaterial: '',
        selectedSize: '',
        selectedColor: '',
        customText: '',
        calculatedPrice: 299, // 初始价格
      };
    },
    methods: {
      playVideo() {
        const video = this.$refs.productVideo;
        video.play();
        this.isVideoPlaying = true;
      },
      resetVideo() {
        const video = this.$refs.productVideo;
        if (video) {
          video.pause();
          video.currentTime = 0;
          this.isVideoPlaying = false;
        }
      },
      showWechatQRCode() {
        this.showQRCode = true;
      },
      selectMaterial(value) {
        this.selectedMaterial = value;
        this.updatePrice();
      },
      selectSize(value) {
        this.selectedSize = value;
        this.updatePrice();
      },
      selectColor(value) {
        this.selectedColor = value;
        this.updatePrice();
      },
      updatePrice() {
        // 根据选择的规格组合匹配价格
        const selectedSpec = this.product.specs.find(
          spec =>
            spec.material === this.selectedMaterial &&
            spec.size === this.selectedSize &&
            spec.color === this.selectedColor
        );

        // 如果找到匹配的规格组合，更新价格
        if (selectedSpec) {
          this.calculatedPrice = selectedSpec.price;
        } else {
          // 如果没有匹配的规格组合，显示默认价格
          this.calculatedPrice = this.product.price;
        }
      },
      handleBuy() {
        // 处理立即购买逻辑
      },
      handleAddToCart() {
        // 处理加入购物车逻辑
      },
      handleAddToWishlist() {
        // 处理收藏逻辑
      },
      shareToWechat() {
        // 处理分享到微信逻辑
      },
      shareToWeibo() {
        // 处理分享到微博逻辑
      },
      shareToXiaohongshu() {
        // 处理分享到小红书逻辑
      },
      viewProduct(productId) {
        this.$router.push(`/product/${productId}`);
      },
    },
  };
  </script>
  
  <style scoped>
  .product-page {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  
  .product-info {
    display: flex;
    gap: 20px;
  }
  
  .product-gallery {
    flex: 1;
    border-radius: 8px;
    overflow: hidden;
  }
  
  .product-gallery .product-image {
    width: 100%;
    height: 500px;
    object-fit: cover;
  }
  
  .video-container {
    position: relative;
  }
  
  .product-video {
    width: 100%;
    height: 500px;
    object-fit: cover;
  }
  
  .video-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
  }
  
  .video-play-button {
    font-size: 50px;
    color: white;
  }
  
  .product-details {
    flex: 1;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
  }
  
  .price-section {
    margin: 20px 0;
  }
  
  .price {
    font-size: 24px;
    font-weight: bold;
    color: #e4393c;
  }
  
  .discount {
    font-size: 16px;
    color: #999;
    text-decoration: line-through;
  }
  
  .stock-status {
    margin: 20px 0;
  }
  
  .actions {
    margin: 20px 0;
  }
  
  .actions .el-button {
    margin-right: 10px;
  }
  
  .social-share {
    margin: 20px 0;
  }
  
  .social-share .el-button {
    margin-right: 10px;
  }
  
  .product-specs {
    margin-bottom: 40px;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
  }
  
  .specs-container {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  
  .specs-group h3 {
    margin-bottom: 10px;
    font-size: 16px;
    font-weight: bold;
  }
  
  .specs-options {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
  }
  
  .specs-options el-button {
    min-width: 80px;
  }
  
  .product-description {
    margin-bottom: 40px;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
  }
  
  .product-highlights {
    margin: 20px 0;
  }
  
  .product-highlights ul {
    list-style: disc;
    padding-left: 20px;
  }
  
  .product-images {
    display: flex;
    gap: 20px;
    margin: 20px 0;
  }
  
  .product-images img {
    width: 100%;
    height: 300px;
    object-fit: cover;
    border-radius: 8px;
  }
  
  .product-reviews {
    margin-bottom: 40px;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
  }
  
  .rating-section {
    display: flex;
    align-items: center;
    gap: 10px;
    margin: 20px 0;
  }
  
  .review-count {
    color: #666;
  }
  
  .reviews {
    margin: 20px 0;
  }
  
  .review {
    margin-bottom: 20px;
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .review-header {
    display: flex;
    align-items: center;
    gap: 10px;
  }
  
  .review-text {
    margin: 10px 0;
  }
  
  .review-images {
    display: flex;
    gap: 10px;
    margin: 10px 0;
  }
  
  .review-images img {
    width: 100px;
    height: 100px;
    object-fit: cover;
    border-radius: 8px;
  }
  
  .related-products {
    margin-bottom: 40px;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
  }
  
  .product-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 20px;
  }
  
  .product-card {
    text-align: center;
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .product-card .product-image {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: 8px;
  }
  
  .product-card h3 {
    margin: 10px 0;
  }
  
  .product-card .product-price {
    font-size: 18px;
    font-weight: bold;
    color: #e4393c;
    margin: 10px 0;
  }
  
  .product-card .el-button {
    margin-top: 10px;
  }
  
  .qr-code-container {
    text-align: center;
  }
  
  .qr-code {
    width: 200px;
    height: 200px;
    margin-bottom: 20px;
  }
  </style>
  