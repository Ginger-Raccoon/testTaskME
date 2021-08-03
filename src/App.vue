<template>
  <div class="page" id="app">
    <h1 class="page__title">Форма подачи заявки в отдел сервиса и качества</h1>
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
.page {
  font-family: Avenir, Helvetica;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  max-width: 920px;
  margin: 0 auto;
  padding-top: 64px;
}
.page__title{
  text-align: left;
  font-size: 21px;
  margin: 0 0 22px;
}
</style>
