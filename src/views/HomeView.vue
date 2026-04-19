<script setup>
import FlipCard from '@/components/elemet/FlipCard.vue'
import FlipCardClick from '@/components/elemet/FlipCardClick.vue';
import { ref } from "vue";

const pokerNumbers = ref([1, 1, 1, 1, 1])
const loveCount = ref(0);

// 初始化为 true (正面/背面朝上，取决于你 FlipCard 的定义，通常 true 为正面)
// 假设 #front 是牌背，#back 是牌面。如果想一开始看到牌背，isFlipped 应为 true (显示 front)
const cardStates = ref([true, true, true, true, true]); 

//  动画锁，防止重复点击
const isAnimating = ref(false);
// 生成随机扑克牌数组
const getRandomPokers  = () => {
  const total = 55
  const newPokers = []

  loveCount.value += 1

   if (loveCount.value === 5) {
    // 稍微延迟一点 alert，让 UI 先渲染完之前的操作❤️
    // setTimeout(() => alert("老婆你在干什么"), 100)
    return [30, 27, 36, 54, 55]
  }

  while (newPokers.length < 5) {
    const num = Math.floor(Math.random() * total) + 1
    if (!newPokers.includes(num)) {
      newPokers.push(num)
    }
  }
   
   return newPokers
}

// 将扑克牌数组放入数组
const drawPokers = () => { 
  cardStates.value = [true, true, true, true, true]
  const newPokers = getRandomPokers()
  pokerNumbers.value = newPokers
}

// 开局就换牌
drawPokers()

// 获取图片路径
const getPokerImage = (num) => {
  // 假设图片在 src/assets/img/puke/ 目录下
  // new URL 第二个参数 import.meta.url 告诉构建工具相对于当前文件解析
  return new URL(`../assets/img/puke/${num}.png`, import.meta.url).href
}

const startSequenceFlip = async () => {
  if (isAnimating.value) return
  isAnimating.value = true

  const trueCount = cardStates.value.filter(Boolean).length;

  if (trueCount === 0) {
    drawPokers()
  }

  
  // 遍历每张牌，依次翻转
  for (let i = 0; i < cardStates.value.length; i++) {
    // 延迟 300ms
    await new Promise(resolve => setTimeout(resolve, 300))
    // 切换第 i 张牌的状态
    cardStates.value[i] = !cardStates.value[i]
  }

  isAnimating.value = false
}
</script>

<template>
  <div style="text-align: center; margin-top: 40px;">
    <h1 style="font-size: 30px;">WelCome My Frends!</h1>

     <!-- 控制按钮组 -->
    <div style="margin-bottom: 30px;margin-top: 30px; display: flex; justify-content: center; gap: 20px;">
      <!-- <button 
        @click="drawPokers" 
        style="padding: 12px 30px; font-size: 16px; background: #d82c2c; color: white; border: none; border-radius: 8px; cursor: pointer;"
      >
        重新抽牌
      </button> -->

      <button 
        @click="startSequenceFlip" 
        :disabled="isAnimating"
        style="padding: 12px 30px; font-size: 16px; background: #409eff; color: white; border: none; border-radius: 8px; cursor: pointer;"
      >
        {{ isAnimating ? '翻转中...' : '顺序翻牌' }}
      </button>

      <button 
        @click="drawPokers" 
        style="padding: 12px 30px; font-size: 16px; background: #67c23a; color: white; border: none; border-radius: 8px; cursor: pointer;"
      >
        重置牌面
      </button>
    </div>

    <!-- 扑克牌容器 -->
    <div style="display: flex; justify-content: center; gap: 20px; flex-wrap: wrap; max-width: 1200px; margin: 0 auto;">
      <!-- 水平翻转 (默认) -->
      <FlipCardClick 
        v-for="(num, index) in pokerNumbers"
        :key="index"
        v-model="cardStates[index]" 
        :clickable="false" 
        width="200px" 
        height="307px" 
        :duration="1.5" 
        :border-radius="'30px'"
        style="margin-left: 50px; margin-right: 50px;"
      >
        <template #front>
          <div style="text-align: center;">
              <img src="@/assets/img/paper/CardBack2.png" alt="back" style="width: 100%; height: 100%; object-fit: cover;">
          </div>          
        </template>
        <template #back>
          <div style="text-align: center;">
              <img :src="getPokerImage(num)" alt="Joker" style="width: 100%; height: 100%; object-fit: cover;">
          </div>
        </template>
      </FlipCardClick>
    </div>
    

    <div style="background-color: azure; height: 500px; border-radius: 30px; margin-top: 50px;">

    </div>
    
  </div>

 
</template>
