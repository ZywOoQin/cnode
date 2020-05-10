<template>
  <div>
    <div class="loading" v-if="isloading">
      <img src="../assets/loading.gif">
    </div>
    <div id="slidebar" v-else>
      <div class="panel">
        <div class="header">个人信息</div>
        <div class="inner">
          <div class="user_card">
            <a href="javascript:;" class="user_avatar">
              <img :src="slide.avatar_url" :title="slide.loginname">
            </a>
            <span class="user_name">
              <a href="javascript:;" class="dark">{{slide.loginname}}</a>
            </span>
            <div class="board">
              <span>积分：{{slide.score}}</span>
            </div>
            <span class="signature">“这家伙很懒”</span>
          </div>
        </div>
      </div>
      <div class="panel">
        <div class="header">作者最近主题</div>
        <div class="inner">
          <ul>
            <li v-for="(item,index) in slide.recent_topics.slice(0 , 5)" :key="index">
              <router-link :to="{name:'article_content',params:{id:item.id,name:item.author.loginname}}">
                <a href="javascript:;">{{item.title}}</a>
              </router-link>
            </li>
          </ul>
        </div>
      </div>
      <div class="panel">
        <div class="header">作者最近回复</div>
        <div class="inner">
          <ul>
            <li v-for="(item,index) in slide.recent_replies.slice(0 , 5)" :key="index">
              <router-link :to="{name:'article_content',params:{id:item.id,name:item.author.loginname}}">
                <a href="javascript:;">{{item.title}}</a>
              </router-link>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SlideBar',
  data () {
    return {
      isloading: false,
      slide:{}
    }
  },
  methods:{
    getSlideBarData(){
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
      .then(res => {
        this.isloading=false;
        this.slide = res.data.data
      })
      .catch(err => {
        console.log(err)
      })
    }
  },
  watch:{
    $route(to,from){
      this.getSlideBarData()
    }
  },
  beforeMount:function(){
    this.isloading=true;
    this.getSlideBarData()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #slidebar {
    width: 290px;
    font-size: 14px;
    float: right;
    margin-bottom: 20px;
  }
  #sidebar .panel {
    font-size: 13px;
    margin-bottom: 13px;
  }
  .header{
    color: #51585c;
    border-radius: 5px 5px 0 0;
    padding: 10px;
    background-color: #fcfcfc;
  }
  .panel .inner{
    padding: 10px;
    border-top: 1px solid #e5e5e5;
  }
  .user_avatar img{
    width: 50px;
    vertical-align: middle;
    margin-right: 10px;
  }
  .user_name{
    max-width: 120px;
    white-space: nowrap;
    display: inline-block;
    vertical-align: middle;
  }
  a.dark:hover{
    color: #385f8a;
  }
  a.dark{
    color: #666;
    text-decoration: none;
  }
  .board {
    margin: 10px 0;
    width: 80%;
  }
  .signature {
    font-style: italic;
  }
  .inner ul li{
    padding: 10px;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space:nowrap;
  }
  .inner li a:hover{
    text-decoration: underline;
  }
</style>
