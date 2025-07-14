<script setup>
  import { ref } from 'vue';  
  import Query from './components/Query.vue';  
  import Favorites from './components/Favorites.vue';  
  
  const currentView = ref('query'); 
</script>

<template>
 <div class="pt-4 pl-8 text-6xl text-blue-700  font-bold ">台北市 Youbike 即時資訊</div>      
  <div class="p-8">      
    <div class="flex gap-2">
      <button @click="currentView = 'query'" class="btn btn-accent btn-lg">  
        查詢站點  
      </button>  
      <button @click="currentView = 'favorites'" class="btn btn-accent btn-lg">  
        收藏站點
      </button> 
      </div>   
      <div class="divider"></div>    
      <Query v-if="currentView === 'query'" />  
      <Favorites v-if="currentView === 'favorites'" />  
   
  </div>  
</template>
<style scoped>

</style>
