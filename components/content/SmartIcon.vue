<template>
  <!-- Iconify Icons -->
  <Icon v-if="checkIcon(name)" :name :size />
  <!-- Emojis -->
  <span
    v-else-if="/(\u00a9|\u00ae|[\u2000-\u3300]|\ud83c[\ud000-\udfff]|\ud83d[\ud000-\udfff]|\ud83e[\ud000-\udfff])/g.test(name)"
    :style="`font-size: ${size}px;`"
  >{{ name }}</span>
  <!-- Link -->
  <NuxtImg
    v-else
    :src="name"
    :style="`width: ${size}px; height: ${size}px;`"
    class="inline"
  />
</template>

<script setup lang="ts">
import { stringToIcon, validateIconName } from '@iconify/utils';

const { size = 16 } = defineProps<{
  name: string;
  size?: number;
}>();

function checkIcon(name: string): boolean {
  if (name.includes('http'))
    return false;
  
  // Check if it's a file path (starts with / or contains file extension)
  if (name.startsWith('/') || /\.(png|jpg|jpeg|gif|svg|webp|ico)$/i.test(name))
    return false;

  return validateIconName(stringToIcon(name));
}
</script>
