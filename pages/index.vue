<template>
    <div class="h-screen relative overflow-hidden">
        <img src="" alt="">
        <div class="absolute w-full h-full top-0 overlay">
        </div>
        <div class="absolute w-full h-full top-0 p-48">
            <WeaterInfo
            v-if="data"
            :weather-data="{
                cityName:data.city.name,
                country:data.city.country,
                date:date,
                imageIcon:data.list[0].weather[0].icon,
                temprature:data.list[0].main.temp,
            }"
            />
            <div v-else>找不到該地區的天氣資料</div>
            <div class="mt-20">
                <input type="text" class="w-1/2 h-10 pl-5" placeholder="Search a City" v-model="searchInput" @keyup.enter = "handleWeatherSearch">
                <button class="bg-sky-400 w-20 text-white h-10" @click="handleWeatherSearch">Search</button>
            </div>
        </div>
    </div>
</template>
<script lang="ts" setup>
import { onMounted } from 'vue'
import { useFetch, useRuntimeConfig } from '#app'
const config = useRuntimeConfig();
const date = ref(new Date())
const searchInput = ref<string>("")
const city = ref<string>("Toronto")
const myApiKey = config.WEATER_API_KEY
const weatherApi = computed(()=>{
    return `https://api.openweathermap.org/data/2.5/forecast?q=${city.value}&units=metric&appid=${myApiKey}`
})
const { data, error } =useFetch(()=>weatherApi.value) //使用箭頭函式寫法，只要weatherApi有變化，就會自動執行，並回傳新網址
function handleWeatherSearch(){
    city.value = searchInput.value
}
onMounted(()=>{
    setInterval(()=>{
        date.value = new Date()
    },1000)
})
</script>