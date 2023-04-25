<script setup lang="ts">
// https://github.com/inocan-group/vue3-google-map#custom-control
import { GoogleMap, Marker, MarkerCluster } from 'vue3-google-map'
import { ref } from 'vue'

const GOOGLE_MAP_API = import.meta.env.VITE_GOOGLE_MAP

const center = { lat: 23.5564273, lng: 120.9485428 }

const areaList = [
  {
    id: 1,
    name: '北區',
    sites: [
      {
        position: { lat: 25.047373, lng: 121.5251267 },
        label: '北',
        title: '台北火車站',
        id: 1,
        img: 'https://image.kkday.com/v2/image/get/h_650%2Cc_fit/s1.kkday.com/product_29573/20190814084824_dZzdq/jpg'
      },
      {
        position: { lat: 24.9889248, lng: 121.3118651 },
        label: '桃',
        title: '桃園火車站',
        id: 2,
        img: 'https://lh6.googleusercontent.com/-ciTarCvjPig/Ubhlvcpwn5I/AAAAAAAAKZA/YN4ONlUKSYw/w1298-h715-no/TYS.JPG'
      },
      {
        position: { lat: 24.8015877, lng: 121.9620611 },
        label: '竹',
        title: '新竹火車站',
        id: 3,
        img: 'https://tourism.hccg.gov.tw/tourism555/program_img/hot/hotL/2021011416540532950.jpeg'
      }
    ]
  },
  {
    id: 2,
    name: '中區',
    sites: [
      {
        position: { lat: 23.737241, lng: 121.6152351 },
        label: '投',
        title: '南投火車站',
        id: 4,
        img: 'https://www.taiwan.net.tw/pic.ashx?qp=1/big_scenic_spots/pic_C100_250_17.jpg&sizetype=3'
      },
      {
        position: { lat: 24.1248317, lng: 120.630938 },
        label: '中',
        title: '台中火車站',
        id: 5,
        img: 'https://travel.taichung.gov.tw/image/27957?r=637837893158116676'
      }
    ]
  },
  {
    id: 3,
    name: '南區',
    sites: [
      {
        position: { lat: 22.9971249, lng: 120.2101732 },
        label: '南',
        title: '台南火車站',
        id: 7,
        img: 'https://4.bp.blogspot.com/-9fR6I5wm4mU/VpeX4Ux_EbI/AAAAAAAAez8/5COgZiNF8WE/s1600/TNS2.JPG'
      },
      {
        position: { lat: 22.6394973, lng: 120.3000081 },
        label: '雄',
        title: '高雄火車站',
        id: 8,
        img: 'https://images.newtalk.tw/resize_action2/1200/album/news/639/61471d39b6267.jpeg'
      }
    ]
  }
]

interface Site {
  id: number
  title: string
  label: string
  position: { lat: number; lng: number }
  img: string
}
const selectedSitesList = ref<Site[]>()
function selectArea(areaId: number) {
  selectedSitesList.value = areaList.find((item) => item.id === areaId)?.sites
}
const selectedSite = ref<Site | null | undefined>(null)
function selectSite(siteId: number | null) {
  selectedSite.value = siteId ? selectedSitesList.value?.find((item) => item.id === siteId) : null
}
</script>

<template>
  <main class="relative w-screen h-screen">
    <GoogleMap
      :api-key="GOOGLE_MAP_API"
      class="w-full h-full"
      :center="center"
      :zoom="8"
      :zoomControl="false"
      gestureHandling="none"
      :mapTypeControl="false"
      :fullscreenControl="false"
      :streetViewControl="false"
    >
      <MarkerCluster>
        <Marker v-for="item in selectedSitesList" :key="item.id" :options="item" @click="selectSite(item.id)" />
      </MarkerCluster>
    </GoogleMap>
    <!-- Dialog -->
    <div v-if="!!selectedSite" class="absolute top-0 left-0 sm:top-2 sm:left-2 z-20 bg-white w-full sm:w-1/6 rounded-lg shadow-2xl">
      <header class="flex justify-end items-center">
        <span class="p-3 cursor-pointer text-gray-400 hover:text-black" @click="selectSite(null)">✕</span>
      </header>
      <article class="px-5 pb-5">
        <p>
          <span>站點</span>
          <span class="ml-3 font-bold text-yellow-700">{{ selectedSite?.title }}</span>
        </p>
        <p>
          <span>緯度</span>
          <span class="ml-3 font-bold text-yellow-700">{{ selectedSite?.position.lat }}</span>
        </p>
        <p>
          <span>經度</span>
          <span class="ml-3 font-bold text-yellow-700">{{ selectedSite?.position.lng }}</span>
        </p>
        <img :src="selectedSite?.img" :alt="selectedSite?.title" class="block w-11/12 mx-auto mt-4" />
      </article>
    </div>
    <!-- Area List -->
    <aside class="absolute top-0 right-0 z-10 bg-white w-1/6 opacity-75 sm:opacity-100">
      <ul>
        <li
          v-for="area in areaList"
          :key="area.id"
          class="p-4 cursor-pointer hover:bg-gray-200 flex justify-center"
          @click="selectArea(area.id)"
        >
          {{ area.name }}
        </li>
      </ul>
    </aside>
  </main>
</template>

<style>
html,
body {
  margin: 0;
  padding: 0;
}
</style>
