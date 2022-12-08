<template>
  <div class="container">
    <error-message
      v-if="!!this.error"
      :message="this.error"
    />
    <add-form
      :list="this.other_cities"
      v-if="!!this.other_cities.length"
      @submit="addCity"
    />
    <sort-form
      @submit="onSort"
    />
    <weather-table
      :list="this.selected_cities"
      v-if="!!this.selected_cities.length"
      @remove="onRemove"
    />
  </div>
</template>

<script>
  import axios from 'axios'
  import ErrorMessage from "@/components/ErrorMessage";
  import WeatherTable from "@/components/WeatherTable";
  import AddForm from "@/components/AddForm";
  import SortForm from "@/components/SortForm";
  export default {
    components: {SortForm, AddForm, WeatherTable, ErrorMessage},
    mounted() {
      this.selected_date = this.current_date;
      this.fetchWeather();
    },
    watch:{
      citiesLoaded(){
        this.selected_cities = [...this.selected_cities,this.cities[0]]
      },
      selected_cities(){
        this.other_cities = [...this.cities].filter(city => !this.selected_cities.includes(city))
      }
    },
    methods:{
      onSort(keyName){
        this.selected_cities = [...this.selected_cities].sort((city1,city2) => {
          if(typeof city1[keyName] === 'number'){
            return city2[keyName] - city1[keyName]
          }else{
            return city1[keyName].localeCompare(city2[keyName])
          }
        })
      },
      onRemove(id){
        this.selected_cities = this.selected_cities.filter(city => +city.id !== +id)
      },
      fetchWeather(){
        this.cities.forEach((city,index) => {
          axios.get(`https://api.open-meteo.com/v1/forecast?latitude=${city.latitude}&longitude=${city.longitude}&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FMoscow&start_date=${this.selected_date}&end_date=${this.selected_date}`)
              .then(({data}) => {
                this.setWeather(city, data)
                ++index === this.cities.length ? this.citiesLoaded = true : null;
              })
              .catch(({message}) => this.error = message)
        })
      },
      setWeather(city,data){
        city.maxTemp = data.daily.temperature_2m_max[0];
        city.minTemp = data.daily.temperature_2m_min[0];
      },
      addCity(cityId){
        this.selected_cities = [...this.selected_cities,this.cities.find(city => city.id === cityId)]
      }
    },
    data(){
      return {
        citiesLoaded: false,
        error: null,
        current_date: new Date().toISOString().slice(0, 10),
        selected_date: null,
        selected_cities: [],
        other_cities:[],
        cities: [
            {
              id:1111,
              city: 'Berlin',
              latitude: 50.00,
              longitude: 36.17,
              maxTemp: null,
              minTemp: null,
            },
            {
              id:2222,
              city: 'Paris',
              latitude: 48.85,
              longitude: 2.35,
              maxTemp: null,
              minTemp: null,
            },
            {
              id:33333,
              city: 'London',
              latitude: 51.51,
              longitude: -0.13,
              maxTemp: null,
              minTemp: null,
            },
        ]
      }
    }
  }
</script>

<style>
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    background: none;
    border: none;
    color: black;
    font-family: Arial;
    transition: all 0.4s ease 0s;
    outline: none;
  }
  .container{
    display: flex;
    flex-direction: column;
    padding: 20px;
    row-gap: 20px;
    background-color: black;
    align-items: center;
    min-height: 100vh;
    min-width: 100vw;
  }
</style>
