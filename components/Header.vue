<template>
    <!-- 头部 -->
    <header class="header">
        <div class="container">
            <!-- 品牌LOGO -->
            <nuxt-link to="/" class="logo">
            <img src="@/assets/logo.png" alt="品牌LOGO" class="logo-img" />
            </nuxt-link>

            <!-- 移动端菜单按钮 -->
            <el-button class="mobile-menu-button" type="text" @click="toggleMobileMenu">
            <i class="el-icon-menu"></i>
            </el-button>

            <!-- 主导航 -->
            <nav class="nav" :class="{ 'mobile-menu-open': isMobileMenuOpen }">
            <el-menu
                mode="horizontal"
                :default-active="activeIndex"
                @select="handleSelect"
                :collapse="isMobile"
            >
                <el-menu-item index="1"><nuxt-link to="/">首 页</nuxt-link></el-menu-item>
                <el-menu-item index="2"><nuxt-link to="/about">关于我们</nuxt-link></el-menu-item>
                <el-menu-item index="3"><nuxt-link to="/category">商品分类</nuxt-link></el-menu-item>
                <el-menu-item index="4"><nuxt-link to="/blog">资讯 & 博客</nuxt-link></el-menu-item>
                <el-menu-item index="5"><nuxt-link to="/contact">联系我们</nuxt-link></el-menu-item>
            </el-menu>
            </nav>

            <!-- 搜索框 -->
            <div class="search" :class="{ 'mobile-search': isMobile }">
            <el-input
                v-model="searchQuery"
                placeholder="请输入关键词"
                @keyup.enter="handleSearch"
            >
                <el-button slot="append" icon="el-icon-search" @click="handleSearch" />
            </el-input>
            </div>

            <!-- 用户操作 -->
            <div class="user-actions" :class="{ 'mobile-user-actions': isMobile }">
            <el-dropdown v-if="isLoggedIn" trigger="click">
                <span class="el-dropdown-link">
                <el-avatar :src="userAvatar" />
                </span>
                <el-dropdown-menu slot="dropdown">
                <el-dropdown-item>个人中心</el-dropdown-item>
                <el-dropdown-item>订单管理</el-dropdown-item>
                <el-dropdown-item>收藏夹</el-dropdown-item>
                <el-dropdown-item divided @click="handleLogout">退出登录</el-dropdown-item>
                </el-dropdown-menu>
            </el-dropdown>
            <el-button v-else type="text" @click="handleLogin">登录 / 注册</el-button>
            <el-badge :value="cartCount" class="cart-icon">
                <el-button icon="el-icon-shopping-cart" @click="handleCart" />
            </el-badge>
            </div>
        </div>
    </header>
</template>
<script>
export default {
  data() {
    return {
      activeIndex: '1',
      searchQuery: '',
      isLoggedIn: false,
      userAvatar: '',
      cartCount: 0,
      isMobile: false,
      isMobileMenuOpen: false,
    };
  },
  mounted() {
    this.checkMobile();
    window.addEventListener('resize', this.checkMobile);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.checkMobile);
  },
  methods: {
    handleSelect(index) {
      this.activeIndex = index;
    },
    handleSearch() {
      this.$router.push({ path: '/search', query: { q: this.searchQuery } });
    },
    handleLogin() {
      this.$router.push('/login');
    },
    handleLogout() {
      this.isLoggedIn = false;
    },
    handleCart() {
      this.$router.push('/cart');
    },
    checkMobile() {
      this.isMobile = window.innerWidth <= 768;
    },
    toggleMobileMenu() {
      this.isMobileMenuOpen = !this.isMobileMenuOpen;
    },
  },
};
</script>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background-color: var(--primary-color);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
}

.container {
  display: flex;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.logo {
  display: flex;
  align-items: center;
}

.logo-img {
  height: 40px;
  transition: transform 0.3s ease;
}

.logo:hover .logo-img {
  transform: scale(1.1);
}

.nav {
  flex: 1;
  margin-left: 20px;
}

.nav .el-menu {
  background-color: var(--primary-color);
}

.nav .el-menu-item a {
  color: var(--light-color);
}

.nav .el-menu-item.is-active a {
  color: var(--secondary-color);
}

.nav .el-menu-item:hover a {
  color: var(--neutral-color);
}

.search {
  margin-left: 20px;
  width: 300px;
}

.search .el-input {
  background-color: var(--primary-color);
  border-color: var(--neutral-color);
}

.search .el-input__inner {
  color: var(--light-color);
}

.user-actions {
  margin-left: 20px;
  display: flex;
  align-items: center;
}

.user-actions .el-dropdown-link {
  color: var(--light-color);
}

.cart-icon {
  margin-left: 20px;
}

.cart-icon .el-badge__content {
  background-color: var(--secondary-color);
}

/* 移动端样式 */
.mobile-menu-button {
  display: none;
  margin-left: auto;
  color: var(--light-color);
}

.mobile-menu-open {
  display: block;
}

@media (max-width: 768px) {
  .mobile-menu-button {
    display: block;
  }

  .nav {
    display: none;
    position: absolute;
    top: 60px;
    left: 0;
    right: 0;
    background-color: var(--primary-color);
    z-index: 1000;
  }

  .nav.mobile-menu-open {
    display: block;
  }

  .search {
    width: 100%;
    margin: 10px 0;
  }

  .user-actions {
    margin-left: 0;
  }
}
</style> 