<!-- Login.vue -->
<!-- Login.vue -->
<template>
  <div class="container">
    <!-- 背景模糊效果 -->
    <div class="background"></div>
    <!-- 标题 -->
    <h1 class="title">ERP(自动化仓库管理系统)</h1>
    <!-- 登录表单 -->
    <form class="login-form" @submit.prevent="login">
      <!-- 账号输入框 -->
      <div class="form-group">
        <input type="text" class="form-control" v-model="username" placeholder="账号(admin/user1)">
      </div>
      <!-- 密码输入框 -->
      <div class="form-group">
        <input type="password" class="form-control" v-model="password" placeholder="密码(123456)">
      </div>
      <!-- 登录按钮 -->
      <button type="submit" class="btn btn-primary">登录</button>
    </form>
    <!-- 注册链接 -->
    <router-link v-if="!showPermissionsLink" to="/register" class="register-link">还没有账号？点击注册！</router-link>
  </div>
</template>

<script>
import axios from '@/axios/axios';

export default {
  name: 'Login',
  data() {
    return {
      username: '',
      password: '',
      showPermissionsLink: false
    };
  },
  methods: {
    async login() {
      try {
        const response = await axios.post('/user/login', {
          username: this.username,
          password: this.password
        });
        console.log(response);
        if (response.data.code === 200) {
          // 登录成功，设置本地存储
          sessionStorage.setItem("login_username", this.username);
          sessionStorage.setItem("login_role", response.data.data.role);
          sessionStorage.setItem("access_token",response.data.data.token); 
          sessionStorage.setItem("access_role",response.data.data.role); 
          // 跳转到仪表盘页面
          this.$router.push('/dashboard/myinfo');
          // 设置 axios 请求头中的 Authorization 字段
          axios.defaults.headers.common['Authorization'] = `Bearer ${response.data.data.token}`;
        } else {
          // 登录失败
          console.error('Login failed:', response.data.message);
        }
      } catch (error) {
        // 捕获异常
        console.error('An error occurred while logging in:', error);
      }
    }
  }
};
</script>

<style scoped>
/* 去掉默认边距和填充 */
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
}

/* 主容器样式 */
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
  border: none; 
  margin-top: 180px;
}

/* 背景样式 */
.background {
  background-image: url('https://source.unsplash.com/random/1920x1080');
  background-size: cover;
  background-position: center;
  filter: blur(10px); /* 模糊效果 */
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  border: none;
}

/* 标题样式 */
.title {
  color: white;
  font-size: 2.28em;
  margin-bottom: 30px;
  border: none;
}

/* 登录表单样式 */
.login-form {
  background-color: rgba(255, 255, 255, 0.247); /* 透明背景 */
  padding: 30px;
  border-radius: 20px; /* 圆角 */
  text-align: center;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2); /* 阴影效果 */
  border: none; /* 去除边框 */
}

/* 表单组样式 */
.form-group {
  margin-bottom: 20px;
  border: none;
}

/* 输入框样式 */
.form-control {
  width: 100%;
  padding: 10px;
  border: none; /* 去除边框 */
  border-radius: 10px; /* 圆角 */
}

/* 按钮样式 */
.btn {
  padding: 10px 20px;
  border: none; /* 去除边框 */
  border-radius: 20px; /* 圆角 */
  background-color: #007bff9d;
  color: rgb(255, 255, 255);
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn:hover {
  background-color: #0056b3;
}

/* 注册链接样式 */
.register-link {
  color: white;
  text-decoration: none;
  transition: color 0.2s ease;
  margin-top: 12px;
}

.register-link:hover {
  color: #cceeff;
}
</style>
