<template>
    <div class="row gx-3 mt-5">
      <div class="col-md-8" v-if="imagesArr.length > 0">
          <div v-for="image in imagesArr" :key="image.id" :class="['rounded','mb-4',isLoading ? '' : 'run-animation']" :style="{'display': isLoading ? 'none' : ''}">
              <img :src="image">
          </div>
      </div>
    </div>
</template>

<script>
    export default {
        props: ['imagesArr','isLoading']
    }
</script>