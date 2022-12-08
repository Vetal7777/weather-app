<template>
  <div class="error__container">
    <span class="error__ico">!</span>
    {{message}}
  </div>
</template>

<script>
export default {
  name: "error-message",
  props:{
    message: {
      type: String,
      required: true,
    }
  }
}
</script>

<style scoped>
  .error__container{
    display: flex;
    flex-direction: column;
    background-color: #1c1c1c;
    border: 1px solid #2e2e2e;
    padding: 20px;
    border-radius: 10px;
    row-gap: 20px;
    color: white;
    align-items: center;
    font-size: 14px;
    animation-name: showError;
    animation-duration: 0.7s;
    animation-iteration-count: unset;
  }
  .error__ico{
    color: red;
    border-radius: 50%;
    border: 1px solid red;
    min-width: 30px;
    min-height: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  @keyframes showError {
    0%{
      transform: translateX(200px);
    }
    50%{
      transform: translateX(-10px);
    }
    60%{
      transform: translateX(0);
    }
    70%{
      transform: translateX(-10px);
    }
    100%{
      transform: translateX(0);
    }
  }
</style>