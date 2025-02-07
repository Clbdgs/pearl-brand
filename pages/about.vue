<template>
  <div class="about-page">
    <!-- 品牌介绍 -->
    <section class="brand-intro">
      <h1>关于我们</h1>
      <p>{{ brandIntro }}</p>
    </section>

    <!-- 团队介绍 -->
    <section class="team-intro">
      <h2>我们的团队</h2>
      <div class="team-members">
        <div v-for="member in teamMembers" :key="member.name" class="team-member">
          <img :src="member.photo" :alt="member.name" />
          <h3>{{ member.name }}</h3>
          <p>{{ member.position }}</p>
          <p>{{ member.bio }}</p>
        </div>
      </div>
    </section>

    <!-- 品牌发展历程 -->
    <section class="brand-history">
      <h2>品牌发展历程</h2>
      <div class="timeline">
        <div v-for="event in historyEvents" :key="event.year" class="timeline-event">
          <h3>{{ event.year }}</h3>
          <p>{{ event.description }}</p>
        </div>
      </div>
    </section>

    <!-- 品牌理念 -->
    <section class="brand-philosophy">
      <h2>品牌理念</h2>
      <p>{{ brandPhilosophy }}</p>
    </section>

    <!-- 社会责任 -->
    <section class="social-responsibility">
      <h2>社会责任</h2>
      <p>{{ socialResponsibility }}</p>
    </section>

    <!-- CTA -->
    <section class="cta">
      <button @click="contactUs">联系我们</button>
    </section>
  </div>
</template>

<script setup>
const brandIntro = '我们的品牌成立于XXXX年，致力于为年轻男性提供潮流珍珠饰品...';
const teamMembers = [
  { name: '张三', photo: require('@/assets/banner1.png'), position: '创始人', bio: '致力于打造潮流珍珠品牌...' },
  // 其他团队成员
];
const historyEvents = [
  { year: '2018', description: '品牌成立，推出首款珍珠饰品...' },
  // 其他历史事件
];
const brandPhilosophy = '我们相信珍珠不仅是传统的象征，更是现代潮流的代表...';
const socialResponsibility = '我们致力于可持续采购珍珠，支持手工艺人...';

const contactUs = () => {
  // 处理联系我们的逻辑
};
</script>

<style scoped>
.about-page {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.team-members {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}

.team-member {
  text-align: center;
  margin: 20px;
}

.timeline {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.timeline-event {
  margin: 20px 0;
}
</style> 