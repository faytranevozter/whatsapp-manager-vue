<script setup>
  defineProps({
    total: {
      type: Number,
      default: 0
    },
    content: {
      type: String,
      default: ''
    },
    color: {
      type: String,
      default: ''
    },
  })
</script>
<template>
  <div class="flex items-center p-4 bg-white rounded-lg shadow-xs dark:bg-gray-800">
    <div :class="(`p-3 mr-4 rounded-full ${color}`)">
      <slot />
    </div>
    <div>
      <p class="mb-2 text-sm font-medium text-gray-600 dark:text-gray-400">
        {{ content }}
      </p>
      <p class="text-lg font-semibold text-gray-700 dark:text-gray-200">
        {{ total }}
      </p>
    </div>
  </div>
</template>