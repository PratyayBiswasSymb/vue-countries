<template>
    <div class="CountryView">
        <h2>{{ data?.name?.common }}</h2>
        <div class="main" v-if="render">
            <img :src="data.flags.png" alt="">
            <div class="info">
                <div class="row">
                    <div class="left">Native Name:</div>
                    <div class="right">{{ nativeName }}</div>
                </div>
                <div class="row">
                    <div class="left">Capital:</div>
                    <div class="right">{{ data?.capital?.[0] }}</div>
                </div>
                <div class="row">
                    <div class="left">Population:</div>
                    <div class="right">{{ data.population }}</div>
                </div>
                <div class="row">
                    <div class="left">Region:</div>
                    <div class="right">{{ data.region }}</div>
                </div>
                <div class="row">
                    <div class="left">Sub-region:</div>
                    <div class="right">{{ data?.subregion }}</div>
                </div>
                <div class="row">
                    <div class="left">Area:</div>
                    <div class="right">{{ data?.area }} Km&#178;</div>
                </div>
                <div class="row">
                    <div class="left">Country Code:</div>
                    <div class="right">{{ countryCode }}</div>
                </div>
                <div class="row">
                    <div class="left">Languages:</div>
                    <div class="right">{{ languages }}</div>
                </div>
                <div class="row">
                    <div class="left">Currencies:</div>
                    <div class="right">{{ currencies }}</div>
                </div>
                <div class="row">
                    <div class="left">Timezones:</div>
                    <div class="right">{{ timezones }}</div>
                </div>
            </div>
        </div>
        <div class="neighbours" v-if="render">
            <h2>Neighbour Countries</h2>
            <div class="grid">
                <div class="grid-item" v-for="item in data.borders" :key="item">
                    <NeighbourFlag :name="item" />
                </div>
                <!-- <NeighbourFlag v-for="item in data.borders" :key="item" :name="item" /> -->
            </div>
            <!-- <NeighbourFlag key="item.name.official" :flag="countryCode" /> -->
            <!-- <NeighbourFlag v-for="item in data.borders" :key="item" :name="item"/> -->


            <!-- <div class="item"></div>
            <div class="item"></div>
            <div class="item"></div>
            <div class="item"></div> -->

        </div>
    </div>
    <div></div>
</template>
<script setup>

import { onMounted, defineAsyncComponent, ref } from 'vue'
import { useRoute } from "vue-router";
import NeighbourFlag from "../components/NeighbourFlag.vue"


const data = ref({});
const render = ref(false);
let countryCode = "", languages = "", currencies = "", timezones = "";
let borders = [];
let nativeName = "";

onMounted(async () => {
    try {
        const route = useRoute();
        const response = await fetch('https://restcountries.com/v3.1/alpha/' + route.params.country);
        const fetchedData = await response.json();
        data.value = fetchedData[0];
        countryCode = objectsToString(data.value.idd.suffixes)
        languages = objectsToString(data.value.languages)
        currencies = objectsToString(data.value.currencies, "name")
        timezones = objectsToString(data.value.timezones)
        borders = data.value.borders;
        const nm = Object.keys(data.value.name.nativeName);
        nativeName = data.value.name.nativeName[nm[0]].common;
        document.title = data.value.name.common
        render.value = true;
        console.log(data.value);
    } catch (error) {
        console.error('Error fetching data:', error);
    }
});

function objectsToString(d, flag = "") {
    let r = "";
    Object.keys(d).forEach((obj, index) => {
        if (flag.length) {
            r = r + d[obj][flag];
        } else {
            r = r + d[obj];
        }
        if (index == Object.keys(d).length - 2) {
            r = r + " and ";
        } else if (index < Object.keys(d).length - 1) {
            r = r + ", ";
        }
    })
    return r;
}

</script>

<style scoped>
.CountryView {
    margin: 0px auto;
    max-width: 800px;
}

.main {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: var(--margin);
}

img {
    margin-right: var(--margin);
    width: 50%;
    /* max-width: 40%; */
    height: fit-content;
    object-fit: contain;
    border: 0.5px solid var(--color-border);
}

.info {
    width: 50%;
}

.row {
    display: flex;
}

.left {
    margin-right: 4px;
}

.neighbours {
    border: 0.5px solid black;
}

.neighbours h2 {
    margin: var(--margin);
}

.grid {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    /* display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 1rem; */
}

.grid-item {
    max-width: 200px;
    margin: var(--margin);
}

@media screen and (width < 576px) {
    .main {
        flex-direction: column;
    }

    img {
        margin: 0px;
        margin-bottom: var(--margin);
        width: 100%;
        /* max-width: 40%; */
        height: fit-content;
        object-fit: contain;
        border: 0.5px solid var(--color-border);
    }

    .info {
        width: 100%;
    }
}
</style>