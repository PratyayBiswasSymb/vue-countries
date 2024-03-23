<template>
   <a v-if="render" :href="'/' + data.cca3">
      <img :src="data.flags.png" alt="">
   </a>
</template>

<script setup>
import { onMounted, ref } from 'vue'
const props = defineProps(['name'])
const { name } = props;

const data = ref({});
const render = ref(false);

onMounted(async () => {
   try {
      const response = await fetch('https://restcountries.com/v3.1/alpha/' + name);
      const fetchedData = await response.json();
      data.value = fetchedData[0];
      render.value = true;
   } catch (error) {
      console.error('Error fetching data:', error);
   }
});
</script>

<style scoped>
a{
   padding: 0px;
   
}
img {
   /* border: 1px solid black; */
   width: 100%;
   max-width: 200px;
}
</style>