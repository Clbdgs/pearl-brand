<template>
  <div class="comment-section">
    <div class="comments">
      <div v-for="comment in comments" :key="comment.id" class="comment">
        <span class="author">{{ comment.author }}</span>
        <span class="date">{{ comment.date }}</span>
        <p>{{ comment.content }}</p>
      </div>
    </div>
    <h2>评论</h2>
    <el-input
      v-model="newComment"
      type="textarea"
      placeholder="请输入您的评论"
      :rows="3"
    />
    <el-button class="submit-button" type="primary" @click="submitComment">提交评论</el-button>
  </div>
</template>

<script>
export default {
  props: {
    comments: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      newComment: '',
    };
  },
  methods: {
    submitComment() {
      this.$emit('submit-comment', this.newComment);
      this.newComment = '';
    },
  },
};
</script>

<style scoped>
.comment-section {
  margin-top: 40px;
}

.comments {
  margin-top: 20px;
}

.comment {
  border-bottom: 1px solid #eee;
  padding: 10px 0;
}

.author {
  color: #666;
  font-size: 14px;
}

.date {
  color: #999;
  font-size: 12px;
}
.submit-button {
  margin-top: 10px;
}
</style> 