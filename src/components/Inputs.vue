<template>
    <div class="row gx-3">
      <div class="col-md-4">
          <div :class="['components','p-4','rounded','run-animation']">
                <h1 class="text">Enter a date:</h1>
                <input v-model="date" class="form-control" type="date" @change="processData">
          </div>
      </div>
      <div class="col-md-4 mt-4 mt-md-0">
          <div :class="['components','p-4','rounded','run-animation']">
                <h1 class="text">Enter a time:</h1>
                <input v-model="time" class="form-control" @change="processData" type="time" step='1'>
          </div>
      </div>
    </div>
    <div class="row gx-3 mt-2" v-if="!acceptedDate">
        <div class="col-8">
            <div class="alert alert-warning run-animation">Please enter a date and time that is earlier than the current time</div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data(){
        return {
            date: '',
            time: '',
            acceptedDate: true
        }
    },
    computed: {
        processData(){
            if(this.date != '' && this.time != ''){
                var today = new Date()
                var currentDate = today.toISOString().split('T')[0]
                var timestamp = this.date + 'T' + this.time
                var currentTimestamp = currentDate + 'T' + today.toLocaleTimeString()
                var selectedDate = new Date(timestamp + '+08:00')
                this.acceptedDate = selectedDate.getTime() < today.getTime()
                if(!this.acceptedDate){
                    this.date = ''
                    this.time = ''
                    setTimeout(() => {
                        this.acceptedDate = true
                    }, 3000);
                }else{
                    var url = 'https://api.data.gov.sg/v1/transport/traffic-images'
                    var imagesDict = {}
                    var locationDict = {}
                    axios.get(url, {
                        params: {
                            date_time: timestamp
                        }
                    })
                    .then(res => {
                        var cameras = res.data.items[0].cameras
                        axios.get('https://api.data.gov.sg/v1/environment/2-hour-weather-forecast', {
                            params: {
                                date_time: currentTimestamp,
                                date: currentDate
                            }
                        })
                        .then(res => {
                            res.data.area_metadata.forEach(area => {
                                locationDict[area.name] = {
                                    coordinate: [area.label_location.latitude, area.label_location.longitude],
                                    weather: ''
                                }
                            })
                            res.data.items[0].forecasts.forEach(forecast => {
                                locationDict[forecast.area].weather = forecast.forecast
                            })
                            for(const coordinates in locationDict){
                                cameras.forEach(camera => {
                                    if(Math.abs(camera.location.latitude - locationDict[coordinates].coordinate[0]) < 0.01 && Math.abs(camera.location.longitude - locationDict[coordinates].coordinate[1]) < 0.01){
                                        if(coordinates in imagesDict){
                                            var arr = imagesDict[coordinates].images
                                            arr.push(camera.image)
                                            imagesDict[coordinates].images = arr
                                        }
                                        else{
                                            imagesDict[coordinates] = {
                                                images: [camera.image],
                                                weather: locationDict[coordinates].weather
                                            }
                                        }
                                    }
                                })
                            }
                            this.$emit('handleChange', imagesDict)
                        })
                    })
                }
            }
        }
    }
}
</script>