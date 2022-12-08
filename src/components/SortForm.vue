<template>
  <div class="sort-form__container">
    <select
        class="sort-form__select"
        v-model="value"
    >
      <option
          ref="defaultOption"
          disabled
          value="Sort by"
      >
        Sort by
      </option>
      <option
          :value="item"
          v-for="item in list"
      >
        {{item[0].toUpperCase() + item.slice(1)}}
      </option>
    </select>
    <button
        :class="{
          'disable': !this.ready,
        }"
        class="sort-form__submit"
        @click="onSort"
    >
      Sort
    </button>
  </div>
</template>

<script>
export default {
  name: "sort-form",
  data(){
    return {
      ready: false,
      value: 'Sort by',
      list: ['city','maxTemp','minTemp']
    }
  },
  methods:{
    onSort(){
      this.$emit('submit',this.value);
      this.value = 'Sort by'
      this.ready = false;
    }
  },
  watch:{
    value(){
      this.value !== 'Sort by' ? this.ready = true : null
    }
  }
}
</script>

<style scoped>
  .sort-form__container{
    display: flex;
    column-gap: 20px;
    background-color: #1c1c1c;
    border-radius: 10px;
    border: 1px solid #2e2e2e;
    padding: 10px;
    align-items: center;
  }
  .sort-form__select{
    display: flex;
    background-color: #2e2e2e;
    border-radius: 10px;
    cursor: pointer;
    padding: 10px;
    appearance: none;
    min-width: 200px;
    color: white;
  }
  .sort-form__submit{
    color: #0085ff;
    cursor: pointer;
    transform: scale(1);
  }
  .sort-form__submit:hover{
    opacity: 0.5;
  }
  .disable{
    transform: scale(0);
  }
</style>