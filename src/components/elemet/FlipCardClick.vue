<template>
  <div
    class="flip-card"
    :class="[directionClass, { 'is-flipped': !showFront }]"
    :style="cardStyle"
    v-bind="$attrs"
    @click="handleClick"
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
import { computed, ref, watch } from 'vue'

const props = defineProps({
  width: { type: String, default: '300px' },
  height: { type: String, default: '200px' },
  direction: {
    type: String,
    default: 'horizontal',
    validator: (value) => ['horizontal', 'vertical'].includes(value)
  },
  duration: { type: Number, default: 0.6 },
  borderRadius: { type: String, default: '12px' },
  // 新增：外部控制是否显示正面，true为正面，false为反面
  modelValue: { type: Boolean, default: true },
  // 新增：是否允许点击切换，如果外部完全控制，可以设为false
  clickable: { type: Boolean, default: true }
})

const emit = defineEmits(['update:modelValue', 'click'])

// 内部状态，用于同步 prop 的变化
const showFront = ref(props.modelValue)

// 监听外部 prop 变化，同步内部状态
watch(() => props.modelValue, (newVal) => {
  showFront.value = newVal
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
  '--card-border-radius': props.borderRadius
}))

// 内层翻转容器样式
const innerStyle = computed(() => ({
  transition: `transform ${props.duration}s cubic-bezier(0.23, 1, 0.32, 1)`
}))

const handleClick = () => {
  if (!props.clickable) return
  
  // 切换状态
  const newState = !showFront.value
  showFront.value = newState
  
  // 向父组件发射更新事件，实现 v-model 双向绑定
  emit('update:modelValue', newState)
  emit('click', newState)
}
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
  
  border-radius: var(--card-border-radius); 
  
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
  
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
  /* 默认正面不旋转 */
  transform: rotateY(0deg) rotateX(0deg);
}

/* 反面样式 */
.flip-card-back {
  background: linear-gradient(135deg, #161516 0%, #292727 100%);
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
/* 初始状态：反面旋转180度藏在后面 */
.flip-card--horizontal .flip-card-back {
  transform: rotateY(180deg);
}

/* 翻转状态：整个容器旋转180度 */
.flip-card--horizontal.is-flipped .flip-card-inner {
  transform: rotateY(180deg);
}

/* ========== 垂直翻转 (上下) ========== */
/* 初始状态：反面旋转180度藏在后面 */
.flip-card--vertical .flip-card-back {
  transform: rotateX(180deg);
}

/* 翻转状态：整个容器旋转180度 */
.flip-card--vertical.is-flipped .flip-card-inner {
  transform: rotateX(180deg);
}

/* 悬停阴影效果保留，但不再触发翻转 */
.flip-card:hover .flip-card-front,
.flip-card:hover .flip-card-back {
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
}

.flip-card:focus-visible {
  outline: 3px solid #4f46e5;
  outline-offset: 2px;
}
</style>