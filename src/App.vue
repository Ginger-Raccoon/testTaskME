<template>
  <div id="app">
    <h1>Форма подачи заявки в отдел сервиса и качества</h1>
    <TheForm />
  </div>
</template>

<script>
import TheForm from './components/TheForm.vue'

export default {
  name: 'App',
  components: {
    TheForm
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px;

}
</style>
