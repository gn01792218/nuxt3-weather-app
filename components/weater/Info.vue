<template>
    <div class="flex justify-between">
        <div>
            <h1 class="text-7xl text-white">{{ weatherData.cityName }}</h1>
            <h2>{{ weatherData.country }}</h2>
            <p class="font-extralight text-2xl mt-2 text-white">本地時間 : {{ weatherData.date.toLocaleString() }}</p>
            <img class="w-56 icon" :src="`https://openweathermap.org/img/wn/${weatherData.imageIcon}@4x.png`" alt="">
        </div>
        <div>
            <p class="text-9xl text-white font-extralight">{{ weatherData.temprature }}°</p>
        </div>
    </div>
</template>
<script lang="ts" setup>
interface WeaterData {
    cityName: string,
    country: string,
    date: Date,
    imageIcon: string,
    temprature: number,
}
defineProps<{
    weatherData: WeaterData
}>()
</script>