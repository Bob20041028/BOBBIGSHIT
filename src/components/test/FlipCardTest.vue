<script setup>
import FlipCard from './components/elemet/FlipCard.vue'

</script>

<template>

  <div style="display: flex; gap: 24px; flex-wrap: wrap; padding: 40px;">
    <!-- 水平翻转 (默认) -->
    <FlipCard 
      width="280px" 
      height="370px" 
      :duration="1.5" 
      :border-radius="'20px'"
    >
      <template #front>
        <div style="text-align: center;">
          <span style="font-size: 2rem;">李晓琳</span>
        </div>          
      </template>
      <template #back>
        <div style="text-align: center;">
           <img src="@/assets\img\lxl.jpg" alt="Bob" style="width: 100%; height: 100%; object-fit: cover;">
        </div>
      </template>
    </FlipCard>

    <!-- 垂直翻转 + 自定义尺寸与速度 -->
    <FlipCard 
      direction="vertical" 
      width="280px" 
      height="370px" 
      :duration="2.8"
      :border-radius="'20px'"
    >
      <template #front>
        <div style="text-align: center;">
          <span style="font-size: 2rem;">陈锦波</span>
        </div> 
      </template>
      <template #back>
       <div style="text-align: center;">
          <img src="@/assets\img\bob.jpg" alt="Bob" style="width: 100%; height: 100%; object-fit: cover;">
        </div> 
      </template>
    </FlipCard>

  </div>

</template>

<style scoped>  
header {
  font-size: 24px;
  font-weight: bold;
  text-align: center;
  margin-top: 40px;
}
a {
  text-decoration: none;
}

</style>
