<template>
  <div class="header__container">
    <select
        class="header__select"
        v-model="value"
    >
      <option
          ref="defaultOption"
          disabled
          value="Add city to your list"
      >
        Add city to your list
      </option>
      <option
          :value="item.city"
          v-for="item in list"
          :key="list.indexOf(item)"
      >
        {{item.city}}
      </option>
    </select>
    <button
        :class="{
          'disable': !this.ready,
        }"
        class="header__add"
        @click="$emit('submit',this.list.find(item => item.city === this.value).id)"
    >
      Add
    </button>
  </div>
</template>

<script>
export default {
  name: "add-form",
  props:{
    list:{
      type: Array,
      required: true
    }
  },
  data(){
    return {
      ready: false,
      value: 'Add city to your list',
    }
  },
  watch:{
    list(){
      this.value = 'Add city to your list'
      this.ready = false
      this.$refs.defaultOption.selected = true

    },
    value(){
      this.value !== 'Add city to your list' ? this.ready = true : null
    }
  }
}
</script>

<style scoped>
  .header__container{
    display: flex;
    column-gap: 20px;
    background-color: #1c1c1c;
    border-radius: 10px;
    border: 1px solid #2e2e2e;
    padding: 10px;
    align-items: center;
  }
  .header__select{
    display: flex;
    background-color: #2e2e2e;
    border-radius: 10px;
    cursor: pointer;
    padding: 10px;
    appearance: none;
    min-width: 200px;
    color: white;
  }
  .header__add{
    color: #0085ff;
    cursor: pointer;
    transform: scale(1);
  }
  .header__add:hover{
    opacity: 0.5;
  }
  .disable{
    transform: scale(0);
  }
</style>