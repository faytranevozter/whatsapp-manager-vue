<script setup>
import { ref } from "vue";
import axios from "axios";
import {API_BASE_URL} from "../config/api";
import QrcodeVue from "qrcode.vue";
import { TransitionRoot, TransitionChild, Dialog, DialogPanel, DialogTitle } from '@headlessui/vue'
import { TrashIcon } from '@heroicons/vue/20/solid'

const devices = ref([])

const MSG_WAITING_QR = 'Generating qr code'

const qrValue = ref(null)
const qrMessage = ref(MSG_WAITING_QR)

const fetchData = async () => {
  const res = await axios.get(`${API_BASE_URL}/api/v1/device/list`)
  devices.value = res.data.data
}

fetchData()

const requestQR = async () => {
  try {
    const res = await axios.post(`${API_BASE_URL}/api/v1/device/pair`)
    const code = res.data.data.code ?? ''
    qrValue.value = code
  } catch (err) {
    qrMessage.value = err.response.data.message ?? ''
  }
}

const connectDevice = async (deviceID) => {
  const res = await axios.post(`${API_BASE_URL}/api/v1/device/connect`, {
    device_id: deviceID
  })
}

const disconnectDevice = async (deviceID) => {
  const res = await axios.post(`${API_BASE_URL}/api/v1/device/disconnect`, {
    device_id: deviceID
  })
}

const removeDevice = async (deviceID) => {
  if (confirm('Remove this device?')) {
    const res = await axios.post(`${API_BASE_URL}/api/v1/device/remove`, {
      device_id: deviceID
    })

    // refresh data
    fetchData()
  }
}

const toggleConnected = async (deviceID, isConnected) => {
  if (confirm('Toggle connected?')) {
    if (isConnected) {
      await disconnectDevice(deviceID)
    } else {
      await connectDevice(deviceID)
    }
  
    // refresh data
    fetchData()
  }
}

// modal
const isOpen = ref(false)
const setIsOpen = (value) => {
  qrMessage.value = MSG_WAITING_QR
  requestQR()
  isOpen.value = value
}

const closeModal = (value) => {
  setTimeout(() => {
    qrValue.value = null
  }, 200);
  isOpen.value = false

  fetchData()
}

</script>

<template>
  <h2 class="my-6 text-2xl font-semibold text-gray-700 dark:text-gray-200">
    List Device
  </h2>

  <div class="w-full mb-8 overflow-hidden rounded-lg shadow-sm">
    <button
      class="px-4 py-2 text-sm font-medium leading-5 text-white transition-colors duration-150 bg-blue-600 border border-transparent rounded-lg active:bg-blue-600 hover:bg-blue-700 focus:outline-none focus:shadow-outline-blue mb-2"
      @click="setIsOpen(!isOpen)"
    >
      Pair new device
    </button>

    <div class="w-full overflow-x-auto">
      <table class="w-full whitespace-nowrap">
        <thead>
          <tr
            class="text-xs font-semibold tracking-wide text-left text-gray-500 uppercase border-b dark:border-gray-700 bg-gray-100 dark:text-gray-400 dark:bg-gray-800"
          >
            <th class="px-4 py-3">
              Device ID
            </th>
            <th class="px-4 py-3">
              Phone
            </th>
            <th class="px-4 py-3">
              Name
            </th>
            <th class="px-4 py-3">
              Token
            </th>
            <th class="px-4 py-3">
              Connected
            </th>
            <th class="px-4 py-3">
              Action
            </th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y dark:divide-gray-700 dark:bg-gray-800">
          <tr
            v-for="device in devices"
            :key="device.id"
            class="text-gray-700 dark:text-gray-400"
          >
            <td class="px-4 py-3 text-sm">
              {{ device.id }}
            </td>
            <td class="px-4 py-3 text-sm">
              {{ device.phone }}
            </td>
            <td class="px-4 py-3 text-sm">
              {{ device.name }}
            </td>
            <td class="px-4 py-3 text-sm">
              {{ device.token }}
            </td>
            <td class="px-4 py-3 text-sm">
              <span
                :class="{
                  'px-2 py-1 font-semibold leading-tight rounded-full cursor-pointer': true,
                  'text-green-700 bg-green-100 dark:bg-green-700 dark:text-green-100': device.connected,
                  'text-red-700 bg-red-100 dark:bg-red-700 dark:text-red-100': !device.connected,
                }"
                @click="toggleConnected(device.id, device.connected)"
              >
                {{ device.connected ? 'connected' : 'disconnected' }}
              </span>
            </td>
            <td class="px-4 py-3 text-sm">
              <button
                class="flex items-center justify-between px-2 py-2 text-sm font-medium leading-5 text-purple-600 rounded-lg dark:text-gray-400 focus:outline-none focus:shadow-outline-gray"
                aria-label="Delete"
                @click="removeDevice(device.id)"
              >
                <TrashIcon class="w-5 h-5 text-red-500" />
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <TransitionRoot
    appear
    :show="isOpen"
    as="template"
  >
    <Dialog
      as="div"
      class="relative z-10"
      @close="closeModal"
    >
      <TransitionChild
        as="template"
        enter="duration-300 ease-out"
        enter-from="opacity-0"
        enter-to="opacity-100"
        leave="duration-200 ease-in"
        leave-from="opacity-100"
        leave-to="opacity-0"
      >
        <div class="fixed inset-0 bg-black bg-opacity-25" />
      </TransitionChild>

      <div class="fixed inset-0 overflow-y-auto">
        <div
          class="flex min-h-full items-center justify-center p-4 text-center"
        >
          <TransitionChild
            as="template"
            enter="duration-300 ease-out"
            enter-from="opacity-0 scale-95"
            enter-to="opacity-100 scale-100"
            leave="duration-200 ease-in"
            leave-from="opacity-100 scale-100"
            leave-to="opacity-0 scale-95"
          >
            <DialogPanel
              class="w-full max-w-md transform overflow-hidden rounded-2xl bg-white p-6 text-left align-middle shadow-xl transition-all"
            >
              <DialogTitle
                as="h3"
                class="text-lg font-medium leading-6 text-gray-900 text-center"
              >
                Scan QR Below
              </DialogTitle>
              <div class="mt-2">
                <QrcodeVue
                  v-if="qrValue != null"
                  :value="qrValue"
                  size="300"
                  level="L"
                  class="mx-auto"
                />
                <p
                  v-else
                  class="text-sm text-gray-500 text-center italic"
                >
                  {{ qrMessage }}
                </p>
              </div>

              <div class="mt-4 text-center">
                <button
                  type="button"
                  class="inline-flex justify-center rounded-md border border-transparent bg-blue-100 px-4 py-2 text-sm font-medium text-blue-900 hover:bg-blue-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2"
                  @click="closeModal"
                >
                  Done
                </button>
              </div>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>

<style>
</style>
