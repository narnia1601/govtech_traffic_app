<template>
    <div class="row gx-3 mt-5">
      <div class="col-md-8">
          <div :class="['components','p-4','rounded','run-animation']">
            <h5 v-if="Object.keys(imagesDict).length == 0" class="text">Please enter a suitable date and time</h5>
            <div v-else class="row gx-3 gy-3 location">
                <div class="col-md-4" v-for="(images,location,index) in imagesDict" :key="index">
                    <div class="button-component rounded text-white" @click="locationClick(location)">{{ location }}</div>
                </div>
            </div>
          </div>
      </div>
      <div class="col-md-4 mt-4 mt-md-0">
            <div v-if="weather != ''" :class="['components','p-4','rounded','text',isLoading ? '' : 'run-animation']" :style="{'display': isLoading ? 'none' : ''}">
                <h4>The weather at {{ location }} now is:</h4>
                <h3>{{ weather }}</h3>
            </div>
      </div>
    </div>
</template>

<script>
    export default {
        props: ['imagesDict', 'imagesSelected', 'weather', 'location', 'isLoading'],
        methods: {
            locationClick(location){
                this.$emit('handleImagesSelected', {
                    imagesSelected: true,
                    location: location
                })
            }
        }
    }
</script>