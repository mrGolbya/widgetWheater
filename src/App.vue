<script>
import axios from 'axios'

export default {
  data(){
    return{
      city: "",
      error: "",
      info: null,
    }
  },
  computed: {
    cityName(){
      return `городе "${this.city }"`
    },
    showTemp(){
      return `Температура: ${this.info.main.temp}`
    },
    showFeelsLike(){
      return "Ощущается как: "+this.info.main.feels_like
    },
    showTempMin(){
      return "Минимальная температура: "+this.info.main.temp_min
    },
    showTempMax(){
      return "Максимальная температура: "+this.info.main.temp_max
    },
  },
  methods: {
    getWheater(){
      if(this.city.trim().length < 2){
        this.error="Нужно название более одного символа"
        return false
      }
      this.error=""
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=ce93f06ecf1d167b2576508179db414a`)
        .then(res => (this.info = res.data))
    }
  },
}
</script>


<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == '' ? 'вашем городе' : cityName }}</p>
    <input type="text" v-model="city"  placeholder="введите город">
    <button v-if="city !=''" @click="getWheater()">Получить погоду</button>
    <button disabled v-else>Введите название города</button>
    <p class="error">{{ error }}</p>
    <div v-if="info !=null"> 
      <p>{{ showFeelsLike }}</p>
      <p>{{ showTemp }}</p>
      <p>{{ showTempMin }}</p>
      <p>{{ showTempMax }}</p>
    </div>
  </div>
</template>
<style scoped>

</style>
