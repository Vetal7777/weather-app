<template>
  <div class="weather-table__container">
    <div class="weather-table__header">
      <span
          class="weather-table__title"
          :style="{flexBasis: `${100/this.titles.length}%`}"
          v-for="title in titles"
      >
        {{title}}
      </span>
    </div>
    <div class="weather-table__list">
      <weather-item
          :key="item.id"
          @remove="onRemove"
          v-for="item in list"
          :item="item"
          :titles="titles"
      />
    </div>
  </div>
</template>

<script>
import WeatherItem from "@/components/WeatherItem";
export default {
  name: "weather-table",
  components: {WeatherItem},
  props:{
    list:{
      type: Array,
      required:true,
    }
  },
  data(){
    return {
      titles: ['City','MinTemp','MaxTemp'],
    }
  },
  methods:{
    onRemove(id){
      this.$emit('remove',id)
    }
  },
}
</script>

<style scoped>
  .weather-table__container{
    display: flex;
    flex-direction: column;
    border-radius: 10px;
    background-color: #1c1c1c;
    border: 1px solid #2e2e2e;
    overflow: hidden;
    min-width: 500px;
    row-gap: 5px;
  }
  .weather-table__list{
    display: flex;
    flex-direction: column;
    row-gap: 10px;
    padding: 10px;
  }
  .weather-table__header{
    display: flex;
  }
  .weather-table__title{
    display: flex;
    color: #0085ff;
    padding: 20px;
    padding-bottom: 0;
  }
</style>