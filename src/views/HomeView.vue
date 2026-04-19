<script setup>
import FlipCard from '@/components/elemet/FlipCard.vue'
</script>

<template>
  <div style="text-align: center; margin-top: 40px;">
    <h1>欢迎来到我的Vue项目！</h1>
    <p>这是一个使用Vue.js构建的单页面应用程序。</p>

     <!-- 水平翻转 (默认) -->
  <FlipCard 
    width="280px" 
    height="370px" 
    :duration="1.5" 
    :border-radius="'30px'"
  >
    <template #front>
      <div style="text-align: center;">
          <img src="@\assets\img\paper\CardBack2.png" alt="Bob" style="width: 100%; height: 100%; object-fit: cover;">
      </div>          
    </template>
    <template #back>
      <div style="text-align: center;">
          <img src="@\assets\img\paper\Joker.png" alt="Bob" style="width: 100%; height: 100%; object-fit: cover;">
      </div>
    </template>
  </FlipCard>
  
  </div>

 
</template>
