<template>
  <div class="h-screen relative overflow-hidden">
    <img class="w-1/2" :src="bgUrl" alt="" />
    <div class="absolute w-full h-full top-0 overlay"></div>
    <div class="absolute w-full h-full top-0 p-48">
      <WeaterInfo v-if="weater" :weather-data="{
        cityName: weater.city.name,
        country: weater.city.country,
        date: date,
        imageIcon: weater.list[0].weather[0].icon,
        temprature: weater.list[0].main.temp,
      }" />
      <div v-else>
        <p class="text-white">
          找不到該地區的天氣資料
        </p>
        <button class="primary-btn" @click="goBack">Back</button>
      </div>
      <div class="mt-20">
        <input type="text" class="w-1/2 h-10 pl-5" placeholder="Search a City" v-model="searchInput"
          @keyup.enter="handleWeatherSearch" />
        <button class="bg-sky-400 w-20 text-white h-10" @click="handleWeatherSearch">
          Search
        </button>
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { onMounted } from "vue";
import { useRuntimeConfig, useAsyncData } from "#app";
const config = useRuntimeConfig();
const searchInput = ref<string>("");
const cityCookie = useCookie("city");
//假如沒有該cookie，預設就是Taiwan
cityCookie.value = cityCookie.value || 'Taiwan'
const city = ref<string>(cityCookie.value);
const date = ref(new Date());
const bgUrl = ref("");
const myApiKey = config.WEATER_API_KEY;
const weatherApi = computed(() => {
  return `https://api.openweathermap.org/data/2.5/forecast?`;
});
const { data: weater, error } = useAsyncData("getWeatherData",async () => {
    let res;
    try {
      res = await $fetch(weatherApi.value, {
          params: {
            q:city.value,
            units: 'metric',
            appid: myApiKey,
          }
        });
      //撰寫同步邏輯
      const temp = res.list[0].main.temp;
      if (temp <= 20) bgUrl.value = "https://picsum.photos/id/684/600/400";
      else if (temp > 20 && temp <= 25) bgUrl.value = "https://picsum.photos/id/683/600/400";
      else bgUrl.value = "https://picsum.photos/id/81/600/400";
      //設置cookies
      cityCookie.value = city.value;
    } catch (e) {
      alert(e);
    }
    return res;
  },
  {
    watch: [city],
  }
);
function handleWeatherSearch() {
  city.value = searchInput.value;
}
function goBack(){
  searchInput.value = cityCookie.value
  city.value = searchInput.value
}
onMounted(() => {
  setInterval(() => {
    date.value = new Date();
  }, 1000);
});
</script>