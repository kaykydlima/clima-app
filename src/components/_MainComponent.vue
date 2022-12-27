<template>
  <v-container grid-list-s>
    
    <v-text-field v-model.lazy="state.input" label="Search" variant="solo" append-inner-icon="mdi-magnify"
    density="comfortable" @keyup.enter="search" @click:append-inner="search" v-if="state.showInput"></v-text-field>
    
    
    <div v-if="state.isLoading">
      <LoadingComponent />
    </div>
    
    <div class="d-flex justify-center" v-if="state.erro">
      <ErrorComponent />
    </div>
    
    <CardWeather :day="state.day" :obj="state.obj" :dayOne="state.dayOne" v-if="state.showCard" />
  </v-container>

</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios'
import CardWeather from './views/CardWeather.vue';
import ErrorComponent from './views/ErrorComponent.vue'
import LoadingComponent from './views/LoadingComponent.vue'

const state = ref({
  input: '',
  showCard: false,
  erro: false,
  obj:{},
  isLoading: false,
  day: [],
  dayOne: '',
  showInput: true
})

const geolocation = navigator.geolocation
const key = import.meta.env.VITE_KEY_API

geolocation.getCurrentPosition(position => {
  let lat = position.coords.latitude
  let long = position.coords.longitude

  console.log(lat, long)
  axios.get(`https://api.weatherapi.com/v1/forecast.json?key=${key}&q=${lat},${long}&days=1&aqi=yes&alerts=no`)
    .then(resp => {
  state.value.obj = resp.data
  state.value.day = resp.data.forecast.forecastday
  state.value.dayOne = state.value.day[0]
  state.value.isLoading = true
  state.value.showInput = false
  setTimeout(() => {
        state.value.isLoading = false
        state.value.showInput = true
        state.value.showCard = true
      }, "400")
    })
    .catch(e => {
      setTimeout(() => {
        state.value.isLoading = false
        state.value.erro = true
        state.value.showInput = true
      }, "700")
    })
})

function search() {
  state.value.isLoading = true
  state.value.erro = false
  state.value.showCard = false
  state.value.showInput = false 

  axios.get(`https://api.weatherapi.com/v1/forecast.json?key=${key}&q=${state.value.input}&days=1&aqi=yes&alerts=no
`)
    .then(resp => {
      state.value.obj = resp.data
      state.value.day = resp.data.forecast.forecastday
      state.value.input = ''
      state.value.dayOne = state.value.day[0]
      setTimeout(() => {
        state.value.isLoading = false
        state.value.showCard = true
        state.value.showInput = true
      }, "700")
      state.value.input = ''

    })
    .catch(e => {
      console.log(e);
      setTimeout(() => {
        state.value.isLoading = false
        state.value.erro = true
        state.value.showInput = true
      }, "700")
    })

}
</script>