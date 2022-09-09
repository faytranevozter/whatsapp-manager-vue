<script setup>
import { ref } from 'vue';
import axios from "axios";
import {API_BASE_URL} from "../config/api";
import HeadingContent from '../components/HeadingContent.vue';
import CardItem from '../components/CardItem.vue';
import {DevicePhoneMobileIcon} from '@heroicons/vue/24/solid'

const syncData = ref({
  device_quota: 0,
  device_used: 0,
  device_available: 0,
  device_local: 0,
})

const fetchInfo = async () => {
  const res = await axios.get(`${API_BASE_URL}/api/v1/user/info`)
  console.log(res.data)
  syncData.value = res.data.data
}

fetchInfo()
</script>

<template>
  <HeadingContent>Dashboard</HeadingContent>

  <div class="grid gap-6 mb-8 md:grid-cols-2 xl:grid-cols-4">
    <CardItem
      :total="syncData.device_quota"
      content="Total devices"
      color="text-green-500 bg-green-100 dark:text-green-100 dark:bg-green-500"
    >
      <DevicePhoneMobileIcon class="w-5 h-5" />
    </CardItem>

    <CardItem
      :total="syncData.device_used"
      content="Total device used"
      color="text-blue-500 bg-blue-100 dark:text-blue-100 dark:bg-blue-500"
    >
      <DevicePhoneMobileIcon class="w-5 h-5" />
    </CardItem>

    <CardItem
      :total="syncData.device_available"
      content="Total device available"
      color="text-green-500 bg-green-100 dark:text-green-100 dark:bg-green-500"
    >
      <DevicePhoneMobileIcon class="w-5 h-5" />
    </CardItem>

    <CardItem
      :total="syncData.device_local"
      content="Total device in local"
      color="text-orange-500 bg-orange-100 dark:text-orange-100 dark:bg-orange-500"
    >
      <DevicePhoneMobileIcon class="w-5 h-5" />
    </CardItem>
  </div>
</template>
