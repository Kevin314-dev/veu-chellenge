<script setup lang="ts">
const props = defineProps<{
  recipients: string[]
}>()

const recipientsObject = props.recipients
const dataDisplay = recipientsObject[0];




</script>

<template>
  <!-- <span>{{ recipients }}</span> -->
  <span>{{ dataDisplay }}</span>
</template>
