<template>
  <div id="app">
    <FormContainer/>
  </div>
</template>

<script>
import FormContainer from './components/FormContainer.vue'

export default {
  name: 'App',
  components: {
    FormContainer
  }
}
</script>

<style>

* {
  box-sizing: border-box;
}
html, body{
  margin: 0px;
  padding: 0px;

}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  /* background-color: rgb(44, 48, 54); */
  background-image: url("./images/woman_back_dark.png");
  height: 100vh;
  width: 100vw;

  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
