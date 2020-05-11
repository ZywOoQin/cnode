<template>
  <div class="header">
    <router-link :to="{name:'root'}">
      <a href="javascript:;"><img src="../assets/cnodejs_light.svg" alt=""></a>
    </router-link>
    <ul>
      <li>
        <router-link :to="{name:'root'}">
          <a href="javascript:;">首页</a>
        </router-link>
      </li>
      <li><a href="javascript:;">新手入门</a></li>
      <li><a href="javascript:;">API</a></li>
      <li><a href="javascript:;">关于</a></li>
      <li><a href="javascript:;">注册</a></li>
      <li><a href="javascript:;">登录</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Header',
  data () {
    return {
      
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .header{
    height: 50px;
    background-color: #444;
  } 
  img{
    width:120px;
    margin-left: 5%;
    margin-top: 10px;
  }
  ul{
    list-style: none;
    float: right;
    margin: 4px;
    margin-right: 5%;
  }
  li{
    display: inline-block;
    padding: 10px 15px;
  }
  a{
    text-decoration: none;
    text-shadow: none;
    color: #ccc;
    font-size: 13px;
  }
</style>
