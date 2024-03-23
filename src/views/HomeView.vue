<script setup>
// import TheWelcome from '../components/TheWelcome.vue'

import { onMounted, defineAsyncComponent, ref } from 'vue'
import CountryCard from "../components/CountryCard.vue"


const data = ref([]);
// let data = [];

// Fetch data on component mount

onMounted(async () => {
  try {
    const response = await fetch('https://restcountries.com/v3.1/all');
    let fetchedData = await response.json();


    data.value = fetchedData.sort(sortFunction);
    console.log(data.value);
    // data.value = fetchedData;
  } catch (error) {
    console.error('Error fetching data:', error);
  }
});

function sortFunction(a, b) {
  return a.name.common.localeCompare(b.name.common);
}

async function onEnter(e) {
  console.log(e.target.value);
  try {
    const response = await fetch("https://restcountries.com/v3.1/name/" + e.target.value); let fetchedData = await response.json();
    data.value = fetchedData.sort(sortFunction);
    console.log(data.value);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
}
</script>


<template>
  <main>
    <div class="countries">
      <h2>Countries</h2>
      <div class="search-div">
        <input v-on:keyup.enter="onEnter" type="text" placeholder="Search countries">
        <svg xmlns="http://www.w3.org/2000/svg" height="40" viewBox="0 -960 960 960" width="40" fill="grey">
          <path
            d="M779.385-153.846 528.923-404.307q-30 25.538-69 39.538-39 14-78.385 14-96.1 0-162.665-66.529-66.566-66.529-66.566-162.577t66.529-162.702q66.529-66.654 162.577-66.654 96.049 0 162.702 66.565Q610.769-676.101 610.769-580q0 41.692-14.769 80.692-14.769 39-38.769 66.693l250.462 250.461-28.308 28.308ZM381.538-390.769q79.616 0 134.423-54.808Q570.769-500.385 570.769-580q0-79.615-54.808-134.423-54.807-54.808-134.423-54.808-79.615 0-134.423 54.808Q192.308-659.615 192.308-580q0 79.615 54.807 134.423 54.808 54.808 134.423 54.808Z" />
        </svg>
      </div>

      <CountryCard v-for="item in data" :key="item.name.official" :data="item" />
    </div>
  </main>
</template>

<style scoped>
.countries {
  width: 100%;
  max-width: 560px;
  margin: 0px auto;
}

.countries h2 {
  margin-bottom: 0px;
  font-size: 36px;
}
.search-div{
  position: relative;
}
.search-div input {
  margin-bottom: var(--padding);
  padding: var(--padding);
  width: 100%;
  font-size: 18px;
  border: 0.5px solid var(--color-border);
}
.search-div svg{
  position: absolute;
  right: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>