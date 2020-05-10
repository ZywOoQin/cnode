<template>
  <div id="app">
    <Header/>
    <div class="main">
      <router-view name="slide"></router-view>
      <router-view name="main"></router-view>
    </div>
  </div>
</template>

<script>
import Header from './components/Header'
export default {
  name: 'App',
  components :{
    Header
  }
}
</script>

<style>
  *{
    margin: 0;
    padding: 0;
  }
  body{
    background-color: #f6f6f6;
  }
  body, input, p, textarea {
    font-size: 14px;
  }
  a {
    color: #08c;
    text-decoration: none;
  }
  ul,li{
    list-style: none;
  }
  .main{
    width: 90%;
    margin: 20px auto;
  }
</style>
