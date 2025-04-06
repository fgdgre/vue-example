<template>
  <div
    :style="{
      backgroundColor: color,
      border: 'solid 1px black',
      borderRadius: '4px',
      width: '100%',
      paddingBlock: '8px',
      paddingInline: '16px',
    }"
  >
    <div>
      {{ title }}
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  color: string
  title: string
  startDate: string | Date
  endDate: string | Date
}>()
</script>
