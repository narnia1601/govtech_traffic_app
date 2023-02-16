<script setup>
  import Inputs from './components/Inputs.vue'
  import Locations from './components/Locations.vue'
  import Screenshots from './components/Screenshots.vue'
</script>

<template>
  <div :class="['container','mt-5', isLoading ? 'overlay-background' : '']">
    <div class="spinner-border text-primary overlay" v-if="isLoading" role="status"></div>
    <Inputs @handleChange="handleChange"></Inputs>
    <Locations :isLoading="isLoading" :imagesDict="imagesDict" @handleImagesSelected="handleImagesSelected" :location="location" :imagesSelected="imagesSelected" :weather="weather"></Locations>
    <Screenshots :isLoading="isLoading" v-if="imagesSelected" :imagesArr="imagesArr"></Screenshots>
  </div>
</template>

<script>
  export default {
    data(){
      return {
        imagesDict: {},
        imagesSelected: false,
        imagesArr: [],
        weather: '',
        location: '',
        isLoading: false
      }
    },
    methods: {
      handleChange(imagesDict){
        this.isLoading = true
        this.imagesDict = imagesDict
        this.weather = ''
        this.imagesArr = []
        setTimeout(() => {
          this.isLoading = false
        }, 300);
      },
      handleImagesSelected(dictionary){
        this.isLoading = true
        this.imagesSelected = dictionary.imagesSelected
        this.location = dictionary.location
        this.imagesArr = this.imagesDict[this.location].images
        this.weather = this.imagesDict[this.location].weather
        setTimeout(() => {
          this.isLoading = false
        }, 300);
      }
    }
  }
</script>

<style>

</style>
