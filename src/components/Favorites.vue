<script setup>  
  import { ref, onMounted} from 'vue';      
  import axios from "axios";  

  const favorites = ref([]);  
  const stations = ref([]);  
 
  const loadFavorites = async () => {  
    const storedFavorites = localStorage.getItem('favorites');    
    if (storedFavorites) {  
      stations.value = JSON.parse(storedFavorites);  
    }  
    const stationSno = stations.value.map(s => s.sno);    

    try {  
      const { data } = await axios.get("https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json");       
      favorites.value = data.filter(station => stationSno.includes(String(station.sno)));        
    } catch (error) {  
      console.error(error);  
    }  
  };  

  const removeFavorite = (sno) => {    
    const index = favorites.value.findIndex(f => f.sno === sno);     
    favorites.value.splice(index, 1); 
    localStorage.setItem('favorites', JSON.stringify(favorites.value));  
  };    

  onMounted(() => {  
    loadFavorites();  
  }); 

</script> 

<template>  
  <div class="grid grid-cols-1 sm:grid-cols-4 gap-4">  
    <div v-for="station in favorites" :key="station.sno" class="border-1 border-gray-400 border-solid bg-sky-200/30 rounded-lg p-2">  
      <h2 class="card-title">{{ station.sna }}</h2>  
      <p>地區: {{ station.sarea }}</p>  
      <p>地址: {{ station.ar }}</p>  
      <p>可租借車輛數: {{ station.available_rent_bikes }}</p>  
      <p>還車空位數: {{ station.available_return_bikes }}</p>  
      <p>總停車格: {{ station.Quantity }}</p>  
      <div class="justify-end card-actions">  
        <button @click="removeFavorite(station.sno)" class="mr-2 mb-2 btn btn-sm btn-error">  
        取消收藏  
        </button>  
      </div>  
    </div>  
  </div>  
</template>  