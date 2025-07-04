<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import { ref, onMounted } from 'vue'
import { supabase } from '../lib/supabaseClient.js'

const instruments = ref([])
var map

async function getInstruments() {
  const { data, error } = await supabase.from('asukoht_view').select()

  instruments.value = data
}

onMounted(() => {
   getInstruments()

    map = L.map('map').setView([59.22366986251211, 26.01760135372831], 15);
      L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', { maxZoom: 19, attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'}).addTo(map);
})

 async function meetod(asukoht) {
  console.log(map)
 let asukohad = asukoht.slice(11, -1).split(',');
 let kaardile = []
 asukohad = asukohad.slice(0,-1)
 for (const element of asukohad) {
  kaardile.push([Number(element.split(' ')[1].trim()), Number(element.split(' ')[0].trim())]) 
 }
  let polygon = L.polygon(kaardile).addTo(map)
 console.log(asukohad)
      //await supabase.from('testTabel').insert({name: 'INSERT TEST 1', asukoht: 'LINESTRING(26.01760135372831 59.22366986251211, 26.015658189502517 59.22353914272284, 26.016000861695375 59.222532070528956, 26.01744566874285 59.2223726838983, 26.018323868419742 59.2226850056004, 26.01760135372831 59.22366986251211)'})
}

function getAsukoht(asukoht) {
  if (asukoht == null) {
    return '';
  }
  if (asukoht.includes('LINESTRING')) {
    return asukoht.slice(11, -1).split(',');
  }
  return asukoht;
}

</script>

<template>
  <h2>TEST</h2>
  <ul>
    <li v-for="instrument in instruments" :key="instrument.id">{{ instrument.name }} {{ getAsukoht(instrument.asukoht) }} 
      <button v-if="instrument.asukoht?.includes('LINESTRING')" v-on:click=meetod(instrument.asukoht)>NUPP</button>
    </li>
  </ul>
<div id="map"></div>
</template>

<style scoped>
 #map { height: 580px; width: 800px; position: relative;}
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
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

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>