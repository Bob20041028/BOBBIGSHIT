<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div class="app-wrapper">
    <el-container class="layout-container">
      <!-- 头部导航 -->
      <el-header class="app-header">
        <div class="header-content">
          <span class="logo">My Blog</span>
          <nav class="nav-links">
            <RouterLink to="/">首页</RouterLink>
            <RouterLink to="/about">关于</RouterLink>
          </nav>
        </div>
      </el-header>

      <!-- 主体内容区 -->
      <el-main class="app-main">
        <div class="main-content-card">
          <RouterView />
        </div>
      </el-main>

      <!-- 底部页脚 -->
      <el-footer class="app-footer">
        <p>© 2024 My Blog. All rights reserved.</p>
      </el-footer>
    </el-container>
  </div>
</template>

<style scoped>
/* 1. 全局容器：占满视口高度 */
.app-wrapper {
  min-height: 100vh;
}

/* 2. 布局容器：Flex 纵向排列 */
.layout-container {

  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* 3. 头部：固定高度或自适应，内部内容居中限制宽度 */
.app-header {
  /* Element Plus el-header 默认高度为 60px */
  display: flex;
  align-items: center;
  border-bottom: 1px solid #eee; /* 仅保留分隔线以便观察结构 */
}

.header-content {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
}

.nav-links {
  display: flex;
  gap: 20px;
}

/* 4. 主体内容：flex:1 撑开剩余空间 */
.app-main {
  flex: 1; 
  padding: 20px;
  display: flex;
  justify-content: center; /* 让内部卡片水平居中 */
}

.main-content-card {
  width: 100%;
  max-width: 1200px;
  /* 这里不再设置背景色和阴影，仅保留结构 */
}

/* 5. 底部：固定在布局最下方 */
.app-footer {
  text-align: center;
  border-top: 1px solid #eee; /* 仅保留分隔线 */
  padding: 20px 0;
}
</style>