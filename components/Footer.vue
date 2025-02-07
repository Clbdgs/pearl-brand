<template>
  <footer class="footer">
    <div class="container">
      <!-- 网站地图链接 -->
      <div class="footer-links">
        <div class="link-group">
          <h4>关于我们</h4>
          <nuxt-link to="/about" class="link">品牌故事</nuxt-link>
          <nuxt-link to="/about/team" class="link">团队介绍</nuxt-link>
          <nuxt-link to="/about/values" class="link">品牌理念</nuxt-link>
        </div>
        <div class="link-group">
          <h4>帮助中心</h4>
          <nuxt-link to="/help/faq" class="link">常见问题</nuxt-link>
          <nuxt-link to="/help/shipping" class="link">配送政策</nuxt-link>
          <nuxt-link to="/help/returns" class="link">退换货政策</nuxt-link>
        </div>
        <div class="link-group">
          <h4>法律声明</h4>
          <nuxt-link to="/legal/privacy" class="link">隐私政策</nuxt-link>
          <nuxt-link to="/legal/terms" class="link">服务条款</nuxt-link>
          <nuxt-link to="/legal/cookies" class="link">Cookie政策</nuxt-link>
        </div>
        <div class="link-group">
          <h4>联系我们</h4>
          <nuxt-link to="/contact" class="link">在线留言</nuxt-link>
          <nuxt-link to="/contact/locations" class="link">门店地址</nuxt-link>
          <nuxt-link to="/contact/careers" class="link">加入我们</nuxt-link>
        </div>
      </div>

      <!-- 社交媒体链接 -->
      <div class="social-links">
        <a href="https://weibo.com" target="_blank" rel="noopener noreferrer" aria-label="微博">
          <i class="el-icon-s-promotion"></i>
        </a>
        <a href="https://weixin.qq.com" target="_blank" rel="noopener noreferrer" aria-label="微信">
          <i class="el-icon-chat-dot-round"></i>
        </a>
        <a href="https://xiaohongshu.com" target="_blank" rel="noopener noreferrer" aria-label="小红书">
          <i class="el-icon-picture"></i>
        </a>
        <a href="https://instagram.com" target="_blank" rel="noopener noreferrer" aria-label="Instagram">
          <i class="el-icon-camera"></i>
        </a>
      </div>

      <!-- 版权信息 -->
      <div class="copyright">
        &copy; 2023 品牌名称. 保留所有权利.
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: 'Footer',
};
</script>

<style scoped>
.footer {
  background-color: var(--primary-color);
  padding: 40px 20px;
  text-align: center;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

.footer-links {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  margin-bottom: 20px;
}

.link-group {
  margin: 10px 20px;
  text-align: left;
}

.link-group h4 {
  margin-bottom: 10px;
  font-size: 16px;
  font-weight: bold;
}

.link {
  display: block;
  margin: 5px 0;
  color: var(--neutral-color);
  text-decoration: none;
}

.link:hover {
  color: var(--secondary-color);
}

.social-links {
  margin: 20px 0;
}

.social-links a {
  margin: 0 10px;
  color: var(--neutral-color);
  font-size: 24px;
  text-decoration: none;
}

.social-links a:hover {
  color: var(--secondary-color);
}

.copyright {
  margin-top: 20px;
  color: var(--neutral-color);
  font-size: 14px;
}
</style>