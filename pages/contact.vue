<template>
  <div class="contact-page">
    <h1>联系我们</h1>

    <!-- 品牌联系方式 -->
    <section class="contact-info">
      <h2>联系方式</h2>
      <div class="info-list">
        <div class="info-item">
          <i class="el-icon-phone"></i>
          <span>客服热线：</span>
          <a href="tel:400-xxx-xxxx">400-xxx-xxxx</a>
        </div>
        <div class="info-item">
          <i class="el-icon-message"></i>
          <span>品牌邮箱：</span>
          <a href="mailto:support@brand.com">support@brand.com</a>
        </div>
        <div class="info-item">
          <i class="el-icon-location"></i>
          <span>公司地址：</span>
          <a href="https://maps.google.com" target="_blank">上海市浦东新区XX路XX号</a>
        </div>
        <div class="info-item">
          <i class="el-icon-share"></i>
          <span>社交媒体：</span>
          <div class="social-links">
            <a href="#" class="wechat" @mouseover="showWechatQR = true" @mouseleave="showWechatQR = false">
              <i class="el-icon-chat-dot-round"></i>
              <div v-if="showWechatQR" class="qr-code">
                <img src="@/assets/wechat-qr.png" alt="微信二维码" />
              </div>
            </a>
            <a href="#"><i class="el-icon-s-promotion"></i></a>
            <a href="#"><i class="el-icon-picture"></i></a>
            <a href="#"><i class="el-icon-camera"></i></a>
          </div>
        </div>
      </div>
    </section>

    <!-- 在线留言表单 -->
    <section class="contact-form">
      <h2>在线留言</h2>
      <el-form :model="form" :rules="rules" ref="form" label-width="100px">
        <el-form-item label="姓名" prop="name">
          <el-input v-model="form.name" placeholder="请输入您的姓名"></el-input>
        </el-form-item>
        <el-form-item label="联系方式" prop="contact">
          <el-input v-model="form.contact" placeholder="请输入手机号或邮箱"></el-input>
        </el-form-item>
        <el-form-item label="咨询类型" prop="type">
          <el-select v-model="form.type" placeholder="请选择咨询类型">
            <el-option label="售后咨询" value="after-sales"></el-option>
            <el-option label="商务合作" value="business"></el-option>
            <el-option label="其他" value="other"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="留言内容" prop="message">
          <el-input
            type="textarea"
            v-model="form.message"
            :rows="5"
            placeholder="请输入您的留言内容（300字以内）"
            maxlength="300"
            show-word-limit
          ></el-input>
        </el-form-item>
        <el-form-item label="附件">
          <el-upload
            action="https://jsonplaceholder.typicode.com/posts/"
            :limit="3"
            :on-exceed="handleExceed"
          >
            <el-button type="primary">点击上传</el-button>
            <div slot="tip" class="el-upload__tip">支持上传图片或文件，单个文件不超过5MB</div>
          </el-upload>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm">提交</el-button>
          <el-button @click="resetForm">重置</el-button>
        </el-form-item>
      </el-form>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showWechatQR: false,
      form: {
        name: '',
        contact: '',
        type: '',
        message: '',
      },
      rules: {
        name: [
          { required: true, message: '请输入姓名', trigger: 'blur' }
        ],
        contact: [
          { required: true, message: '请输入联系方式', trigger: 'blur' }
        ],
        type: [
          { required: true, message: '请选择咨询类型', trigger: 'change' }
        ],
        message: [
          { required: true, message: '请输入留言内容', trigger: 'blur' }
        ]
      }
    };
  },
  methods: {
    submitForm() {
      this.$refs.form.validate((valid) => {
        if (valid) {
          // 处理表单提交逻辑
          this.$message.success('提交成功！');
        } else {
          return false;
        }
      });
    },
    resetForm() {
      this.$refs.form.resetFields();
    },
    handleExceed() {
      this.$message.warning('最多只能上传3个文件');
    }
  }
};
</script>

<style scoped>
.contact-page {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.contact-info {
  margin-bottom: 40px;
}

.info-list {
  display: grid;
  gap: 20px;
}

.info-item {
  display: flex;
  align-items: center;
  gap: 10px;
}

.social-links {
  display: flex;
  gap: 10px;
  position: relative;
}

.qr-code {
  position: absolute;
  bottom: 30px;
  left: 0;
  background: #fff;
  padding: 10px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
}

.contact-form {
  margin-top: 40px;
}
</style> 