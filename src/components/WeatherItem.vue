<template>
  <div
      class="weather-item__container"
      v-if="!!weatherItem"
  >
    <span
        :style="{flexBasis: `${100/titles.length}%`}"
        class="weather-item__content"
        v-for="title in titles"
    >
      {{weatherItem[title.toLowerCase()]}}
    </span>
    <button
        class="delete"
        @click="$emit('remove',item.id)"
    >
      Delete
    </button>
  </div>
</template>

<script>
export default {
  name: "weather-item",
  props:{
    item:{
      type: Object,
      required: true
    },
    titles:{
      type:Array,
      required: true
    }
  },
  data(){
    return {
      weatherItem: null,
    }
  },
  mounted(){
    this.weatherItem = {}
    const keys = Object
        .keys(this.item)
        .map(key => key.toLowerCase());
    this.titles.forEach(title => {
      this.weatherItem[title.toLowerCase()] = Object.values(this.item)[keys.indexOf(title.toLowerCase())]
    })
  }
}
</script>

<style scoped>
  .weather-item__container{
    background-color: #2e2e2e;
    border-radius: 10px;
    display: flex;
    position: relative;
  }
  .weather-item__container:hover .delete{
    opacity: 1;
  }
  .weather-item__content{
    display: flex;
    padding: 20px;
    flex-grow: 1;
    color: white;
  }
  .delete{
    display: flex;
    align-items: center;
    color: red;
    position: absolute;
    right: 20px;
    min-width: 30px;
    max-height: 30px;
    top: 0;
    bottom: 0;
    margin: auto 0;
    cursor: pointer;
    opacity: 0;
  }
</style>