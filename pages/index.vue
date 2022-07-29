<template>
    <div class="h-screen relative overflow-hidden">
        <img :src="bgUrl" alt="">
        <div class="absolute w-full h-full top-0 overlay">
        </div>
        <div class="absolute w-full h-full top-0 p-48">
            <WeaterInfo
            v-if="weater"
            :weather-data="{
                cityName:weater.city.name,
                country:weater.city.country,
                date:date,
                imageIcon:weater.list[0].weather[0].icon,
                temprature:weater.list[0].main.temp,
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
import { useFetch, useRuntimeConfig, useAsyncData } from '#app'
const config = useRuntimeConfig();
const date = ref(new Date())
const bgUrl = ref("")
const searchInput = ref<string>("")
const city = ref<string>("Toronto")
const myApiKey = config.WEATER_API_KEY
const weatherApi = computed(()=>{
    return `https://api.openweathermap.org/data/2.5/forecast?q=${city.value}&units=metric&appid=${myApiKey}`
})
const { data:weater, error} = useAsyncData('getWeatherData',async ()=>{
    const res = await $fetch(weatherApi.value)
    const temp = res.list[0].main.temp
    if(temp <= 20) bgUrl.value = "https://picsum.photos/id/684/600/400"
    else if (temp > 20 && temp <= 25) bgUrl.value = "https://picsum.photos/id/683/600/400"
    else bgUrl.value = "https://picsum.photos/id/81/600/400"
    return res
},{
    watch:[city]
})
function handleWeatherSearch(){
    city.value = searchInput.value
}
onMounted(()=>{
    setInterval(()=>{
        date.value = new Date()
    },1000)
})
</script>