<template>
  <div class="page-container">
    <el-button mb-2 @click="toggle">Switch Language</el-button>
    <br />

    <el-config-provider :locale="locale">
      <el-table mb-1 :data="[]" />
      <el-pagination :total="100" />
    </el-config-provider>
  </div>
</template>

<script>
// 选项式 API 写法
import zhCn from 'element-plus/es/locale/lang/zh-cn'
import en from 'element-plus/es/locale/lang/en'

export default {
  data() {
    return {
      language: 'zh-cn'
    }
  },
  computed: {
    locale() {
      return this.language === 'zh-cn' ? zhCn : en
    }
  },
  methods: {
    toggle() {
      this.language = this.language === 'zh-cn' ? 'en' : 'zh-cn'
    }
  }
}
</script>

<style scoped>
/* 深色背景 + 全局白色字体 */
.page-container {
  background-color: #1a1a1a;
  min-height: 100vh;
  padding: 20px;
  color: #fff !important;
}

/* 强制 Element Plus 组件文字白色 */
:deep(.el-button),
:deep(.el-table),
:deep(.el-table *),
:deep(.el-pagination *),
:deep(.el-config-provider *) {
  color: #ffffff !important;
  border-color: #444 !important;
}

:deep(.el-table),
:deep(.el-table__cell) {
  background-color: #252525 !important;
}
</style>