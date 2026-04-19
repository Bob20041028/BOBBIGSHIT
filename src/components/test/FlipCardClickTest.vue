<template>
  <div class="container">
    <!-- 示例1：基本使用，点击切换，内部维护状态 -->
    <h3>示例1：内部状态控制</h3>
    <FlipCardClick>
      <template #front><h1>正面内容</h1></template>
      <template #back><h1>反面内容</h1></template>
    </FlipCardClick>

    <hr />

    <!-- 示例2：外部控制状态 (v-model) -->
    <h3>示例2：外部 v-model 控制</h3>
    <div class="controls">
      <button @click="toggleSingleCard">
        {{ isFront ? '切换到反面' : '切换到正面' }}
      </button>
    </div>
    
    <FlipCardClick 
      v-model="isFront" 
      :clickable="false" 
    >
      <template #front><h1>受控正面</h1></template>
      <template #back><h1>受控反面</h1></template>
    </FlipCardClick>

    <hr />

    <!-- 示例3：序列翻转动画 (5个卡片 + 字符串数组) -->
    <h3>示例3：顺序翻转显示文本</h3>
    <div class="controls">
      <button @click="startSequenceFlip" :disabled="isAnimating">
        {{ isAnimating ? '翻转中...' : '开始顺序翻转' }}
      </button>
      <button @click="resetCards" style="margin-left: 10px;">重置</button>
    </div>

    <div class="card-group">
      <FlipCardClick 
        v-for="(card, index) in cards" 
        :key="index"
        v-model="card.isFront"
        :clickable="false"
        width="120px"
        height="80px"
        class="sequence-card"
        :duration="2.5" 
      >
        <!-- 正面：显示序号 -->
        <template #front>
          <div class="card-content front">
            <span style="background-color: #667eea;">{{ index + 1 }}</span>
          </div>
        </template>
        
        <!-- 反面：显示字符串数组中对应的文本 -->
        <template #back>
          <div class="card-content back">
            <!-- 使用 optional chaining (?.) 防止数组越界报错 -->
            <span style="background-color: #667eea;">{{ textList[index] }}</span>
          </div>
        </template>
      </FlipCardClick>
    </div>
  </div>
</template>

<script>
import FlipCardClick from '../elemet/FlipCardClick.vue'

export default {
  name: 'FlipCardClickTest',
  components: {
    FlipCardClick
  },
  data() {
    return {
      // 示例2的数据
      isFront: true,

      // 示例3的数据
      isAnimating: false,
      
      // 定义要显示的字符串数组
      textList: [
        "晓",
        "琳",
        "大",
        "笨",
        "蛋"
      ],
      
      // 初始化5个卡片的状态
      // 注意：cards 的长度应该与 textList 一致，或者动态生成
      cards: [] 
    }
  },
  created() {
    // 在组件创建时，根据 textList 的长度初始化 cards
    this.cards = this.textList.map(() => ({ isFront: true }))
  },
  methods: {
    // 示例2的方法
    toggleSingleCard() {
      this.isFront = !this.isFront
    },

    // 示例3的方法：开始顺序翻转
    async startSequenceFlip() {
      if (this.isAnimating) return
      this.isAnimating = true

      // 遍历每个卡片，按索引增加延迟时间
      for (let i = 0; i < this.cards.length; i++) {
        // 每个卡片间隔 300ms 翻转
        await new Promise(resolve => setTimeout(resolve, 300))
        // 切换当前卡片的状态
        this.cards[i].isFront = !this.cards[i].isFront
      }

      this.isAnimating = false
    },

    // 示例3的方法：重置所有卡片
    resetCards() {
      this.cards.forEach(card => {
        card.isFront = true
      })
    }
  }
}
</script>

<style scoped>
.container {
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: center;
}

.controls {
  margin-bottom: 10px;
}

/* 示例3的卡片组布局 */
.card-group {
  display: flex;
  gap: 15px;
  justify-content: center;
  flex-wrap: wrap;
}

.sequence-card {
  /* 可选：给序列中的卡片加一点额外样式 */
}

.card-content {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  font-size: 1rem; /* 稍微调小字体以适应单词 */
  font-weight: bold;
  padding: 5px;
  text-align: center;
  word-break: break-word;
}

.front {
  background-color: #667eea;
  color: white;
}

.back {
  background-color: #f5576c;
  color: white;
}

hr {
  width: 100%;
  border: 0;
  border-top: 1px solid #eee;
  margin: 20px 0;
}
</style>