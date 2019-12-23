<template>
  <div id="app">
    <Header></Header>
    <b-row>  
      <b-col sm="6" offset="3">
        <QuestionBox></QuestionBox>
      </b-col>
    </b-row>
    
  </div>
</template>

<script>


import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
