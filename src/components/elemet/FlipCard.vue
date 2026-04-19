<template>
  <div
    class="flip-card"
    :class="directionClass"
    :style="cardStyle"
    v-bind="$attrs"
  >
    <div class="flip-card-inner" :style="innerStyle">
      <!-- 正面 -->
      <div class="flip-card-front">
        <slot name="front">
          <div class="default-content">正面</div>
        </slot>
      </div>
      <!-- 反面 -->
      <div class="flip-card-back">
        <slot name="back">
          <div class="default-content">反面</div>
        </slot>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  width: { type: String, default: '300px' },
  height: { type: String, default: '200px' },
  direction: {
    type: String,
    default: 'horizontal',
    validator: (value) => ['horizontal', 'vertical'].includes(value)
  },
  duration: { type: Number, default: 0.6 },
  borderRadius: { type: String, default: '12px' }
})

const directionClass = computed(() => {
  return props.direction === 'horizontal' 
    ? 'flip-card--horizontal' 
    : 'flip-card--vertical'
})

// 卡片容器样式
const cardStyle = computed(() => ({
  width: props.width,
  height: props.height,
  // 关键：将 borderRadius 设置为 CSS 变量，供子元素使用
  '--card-border-radius': props.borderRadius
}))

// 内层翻转容器样式
const innerStyle = computed(() => ({
  transition: `transform ${props.duration}s cubic-bezier(0.23, 1, 0.32, 1)`
}))
</script>

<style scoped>
.flip-card {
  position: relative;
  perspective: 1200px;
  cursor: pointer;
  user-select: none;
}

.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  will-change: transform;
}

/* 正面与反面通用样式 */
.flip-card-front,
.flip-card-back {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  
  /* 关键修改：直接使用 CSS 变量，而不是 inherit */
  border-radius: var(--card-border-radius); 
  
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
  
  /* 确保内容不溢出圆角 */
  overflow: hidden; 
  
  display: flex;
  align-items: center;
  justify-content: center;
  box-sizing: border-box;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
}

/* 正面样式 */
.flip-card-front {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  transform: rotateY(0deg) rotateX(0deg);
}

/* 反面样式 */
.flip-card-back {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  color: white;
}

/* 默认占位内容样式 */
.default-content {
  font-size: 1.5rem;
  font-weight: bold;
  letter-spacing: 2px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

/* ========== 水平翻转 (左右) ========== */
.flip-card--horizontal .flip-card-back {
  transform: rotateY(180deg);
}

.flip-card--horizontal:hover .flip-card-inner {
  transform: rotateY(180deg);
}

/* ========== 垂直翻转 (上下) ========== */
.flip-card--vertical .flip-card-back {
  transform: rotateX(180deg);
}

.flip-card--vertical:hover .flip-card-inner {
  transform: rotateX(180deg);
}

/* 可选：增加悬停时的阴影效果 */
.flip-card:hover .flip-card-front,
.flip-card:hover .flip-card-back {
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
}

/* 移除点击时的默认轮廓 */
.flip-card:focus-visible {
  outline: 3px solid #4f46e5;
  outline-offset: 2px;
}
</style>