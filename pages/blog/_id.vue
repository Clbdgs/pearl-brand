<template>
  <div class="blog-detail">
    <h1>{{ blog.title }}</h1>
    <div class="meta">
      <span>{{ blog.date }}</span>
      <span>作者: {{ blog.author }}</span>
      <span>阅读量: {{ blog.views }}</span>
      <span>点赞数: {{ blog.likes }}</span>
    </div>
    <div class="content" v-html="blog.content"></div>

    <!-- 评论区 -->
    <CommentSection :comments="blog.comments" @submit-comment="handleSubmitComment" />
    <!-- 相关文章推荐 -->
    <div class="related-blogs">
      <h2>相关文章</h2>
      <div class="blog-list">
        <BlogCard v-for="relatedBlog in relatedBlogs" :key="relatedBlog.id" :blog="relatedBlog" />
      </div>
    </div>

  </div>
</template>

<script>
import BlogCard from '@/components/BlogCard.vue';
import CommentSection from '@/components/CommentSection.vue';

export default {
  components: {
    BlogCard,
    CommentSection,
  },
  data() {
    return {
      relatedBlogs: [],
      blog: {},
    };
  },
  async asyncData({ params }) {
    console.log('params', params);
    // const { data: blog } = await $axios.get(`/api/blogs/${params.id}`);
    // const { data: relatedBlogs } = await $axios.get('/api/blogs', { params: { related: params.id } });
    const blog = {
      id: params.id,
      title: '珍珠搭配指南：打造时尚造型',
      date: '2023-10-01',
      author: '时尚达人',
      views: 1234,
      likes: 56,
      content: '<p>分享珍珠首饰的搭配技巧...</p>',
      comments: [
        {
          id: 1,
          author: '用户A',
          avatar: require('@/assets/banner1.png'),
          date: '2023-10-05',
          content: '文章内容很有帮助，学到了很多珍珠搭配技巧！',
          likes: 12
        },
        {
          id: 2,
          author: '用户B',
          avatar: require('@/assets/banner2.png'),
          date: '2023-10-06',
          content: '感谢分享，期待更多时尚搭配指南！',
          likes: 8
        },
        {
          id: 3,
          author: '用户C',
          avatar: require('@/assets/banner1.png'),
          date: '2023-10-07',
          content: '珍珠搭配真的很提升气质，学到了！',
          likes: 15
        }
      ]
    };

    const relatedBlogs = [
      {
        id: 2,
        title: '2023秋季珍珠潮流趋势',
        date: '2023-09-25',
        author: '潮流观察员',
        views: 987,
        likes: 34,
        content: '不错呀.',
        image: require('@/assets/banner1.png')
      },
      {
        id: 3,
        title: '品牌新品发布会回顾',
        date: '2023-09-20',
        author: '品牌小编',
        views: 1567,
        likes: 78,
        content: '不错呀',
        image: require('@/assets/banner1.png')
      }
    ];

    return { blog, relatedBlogs };
  },
  methods: {
    handleSubmitComment(comment) {
      // 处理提交评论逻辑
    },
  },
};
</script>

<style scoped>
.blog-detail {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.meta {
  display: flex;
  justify-content: space-between;
  color: #999;
  margin: 20px 0;
}

.content {
  line-height: 1.6;
}

.related-blogs {
  margin-top: 40px;
}

.blog-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}
</style> 