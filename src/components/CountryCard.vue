<template>
    <div class="CountryCard">
        <img :src="flag" :alt="data.flags.alt">
        <div class="card-info">
            <div id="name">{{ name }}</div>
            <div id="currency">Currency: {{ currency }}</div>
            <div id="dateTime">Current date and time: {{ dateTime }}</div>
            <div class="buttons-div">
                <button v-on:click="goToMap()">Show Map</button>
                <button v-on:click="goToDetails()">Details</button>
            </div>
        </div>
    </div>
</template>

<script setup>
// import router from '@/router';
import { useRouter } from 'vue-router';

const router = useRouter();
const props = defineProps(['data'])
const { data } = props;

const name = data.name.common;
// const flag = "";
const flag = data.flags.png;
const currency = "currencies" in data ? getCurrencies() : "";
const dateTime = getTime(data.timezones)
const id = data.cca3;

function getCurrencies() {
    let c = Object.keys(data?.currencies);
    let r = "";
    c.map((m, index) => {
        r = r + data.currencies[m].name;
        if (index == c.length - 2) {
            r = r + " and ";
        } else if (index < c.length - 1) {
            r = r + ", ";
        }
    })
    return r;
}

function getTime(timezones) {
    const months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];
    try {
        // let t = timezones.length == 1?timezones[0]:timezones[(timezones.length/2)-1]
        const t = timezones[0];
        const s = t.match(/\d+/g);
        const sign = t.substring(3, 4) === "+" ? 1 : -1;
        const offset = sign * (Number(s[0]) + (Number(s[1]) / 60))
        const d = new Date();
        const utc = d.getTime() + (d.getTimezoneOffset() * 60000);
        const nd = new Date(utc + (3600000 * offset));

        let date = nd.getDate();
        if (date == "1" || date == "21" || date == "31") {
            date = date + "st";
        } else if (date == "2" || date == "22") {
            date = date + "nd";
        } else if (date == "3" || date == "33") {
            date = date + "rd";
        } else {
            date = date + "th";
        }

        let month = months[nd.getMonth()];
        let year = nd.getFullYear();
        let time = nd.getTime();
        // return nd.toLocaleString();
        return date + " " + month + " " + nd.getFullYear() + ", " + nd.getHours() + ":" + nd.getMinutes();
    } catch (error) {
        return 0;
    }

}

function goToDetails() {
    router.push({ path: "/" + id })
}
function goToMap() {
    console.log("GO to MAP");
    window.open(data.maps.googleMaps, '_blank');
}

</script>

<style scoped>
.CountryCard {
    margin: var(--margin) 0px;
    padding: var(--padding);
    min-height: 150px;
    width: 100%;
    /* max-width: 500px; */
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-radius: 4px;
    border: 0.5px solid var(--color-border);
    box-shadow: 0px 2px 4px 0px lightgrey;
}

img {
    margin-right: 8px;
    /* width: 40%; */
    max-width: 40%;
    max-height: 120px;
    object-fit: fill;
    border: 0.5px solid var(--color-border);
}

.card-info {
    min-height: 120px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    width: 60%;
    font-size: 13px;
    font-weight: 500;
}

#name {
    font-size: 22px !important;
    font-weight: 500;
    line-height: 34px;
}

#currency,
#dateTime {
    padding-bottom: 4px;
}

.buttons-div {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.buttons-div button {
    width: 48%;
    padding-top: 8px;
    padding-bottom: 8px;
    color: blue;
    font-size: 18px;
    font-weight: 600;
    border: 2px solid blue;
    background-color: white;
}

@media screen and (width < 576px) {
    .CountryCard {
        flex-direction: column;
    }

    img {
        width: unset;
        max-width: 100%;
        max-height: unset;
    }

    .card-info {
        width: 100%;
        max-width: unset;
        min-height: unset;
    }
}
</style>