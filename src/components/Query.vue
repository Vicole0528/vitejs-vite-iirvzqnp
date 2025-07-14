<script setup>
  import { ref, onMounted } from "vue"
  import axios from "axios"
  import Favorites from './Favorites.vue';

  const keyword =ref("");
  const filteredStations = ref([]);  
  const favorites = ref([]);  
  
  const onSearch = async() => {    
    const { data } = await axios.get("https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json");  

    if (keyword.value != "" ) 
    { 
      filteredStations.value = data.filter(item => {  
        return item.sna.includes(keyword.value) || item.ar.includes(keyword.value) || item.sarea.includes(keyword.value);  
      });        
    } else {
      filteredStations.value = data;
    } 
  }

  const isFavorite = (station) => {  
    return favorites.value.some(f => f.sno === station.sno);  
  };  

  const toggleFavorite = (station) => {  
    const index = favorites.value.findIndex(f => f.sno === station.sno);  
    if (index === -1) {  
      favorites.value.push(station);  
    } else {  
      favorites.value.splice(index, 1);  
    }  
    localStorage.setItem('favorites', JSON.stringify(favorites.value));  
  };  

  const loadData = () => {  
    const storedFavorites = localStorage.getItem('favorites');  
    if (storedFavorites) {  
      favorites.value = JSON.parse(storedFavorites);  
    }     
  };  

  onMounted(() => { 
    loadData();  
  }); 



</script>

<template>
  <div>
    <div class="flex gap-2 pb-4">
      <label class="label">輸⼊關鍵字</label>
      <input type="text" @keyup.enter="onSearch" v-model.trim="keyword" class="input" />
      <button @click="onSearch" class="btn">搜尋</button>
    </div>
    
    
      <div class="grid grid-cols-1 sm:grid-cols-4 gap-4">
        <div v-for="station in filteredStations" :key="station.sno" class="border-1 border-gray-400 border-solid rounded-lg p-2">  
          <h2 class="card-title">{{ station.sna }}</h2>  
          <p>地區: {{ station.sarea }}</p>  
          <p>地址: {{ station.ar }}</p>  
          <p>可租借車輛數: {{ station.available_rent_bikes }}</p>  
          <p>還車空位數: {{ station.available_return_bikes }}</p>  
          <p>總停車格: {{ station.Quantity }}</p>  

      
          <div class="justify-end card-actions">              
          <div class="justify-end card-actions">  
          <button @click="toggleFavorite(station)" class ="mr-2 mb-2" :class="isFavorite(station) ? 'btn btn-sm btn-error' : 'btn btn-sm btn-warning'" >  
            {{ isFavorite(station) ? "取消收藏" : "收藏" }}  
          </button>  
          
        </div>  
          </div>  
        </div>
      </div>

</div>
</template>

<style scoped>

</style>
