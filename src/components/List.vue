<template>
  <div class="row">
    <div v-show="info.users.length" class="card" v-for="user in info.users" :key="user.login">
      <a :href="user.html_url" target="_blank">
        <img :src="user.avatar_url" style='width: 100px'/>
      </a>
      <p class="card-text">{{user.login}}</p>
    </div>
    <h1 v-show="info.isFirst">欢迎使用！</h1>
    <h1 v-show="info.isLoading">加载中....</h1>
    <h1 v-show="info.errMsg">{{info.errMsg}}</h1>
  </div>
</template>

<script>
import PubSub from 'pubsub-js'
export default {
name:'List',
	data() {
			return {
				info:{
					isFirst:true,
					isLoading:false,
					errMsg:'',
					users:[]
				}
			}
		},
methods:{
 getUsers(_,value){
    this.info={...this.info,...value}
 }
}
,
mounted(){
  // ?组件一挂载就绑定事件
  // this.$bus.$on('get-data',this.getUsers)
  //?组件一挂载就订阅消息
 this.token = PubSub.subscribe('get-data', this.getUsers);
},
beforeDestroy(){
  //? 组件销毁前取消绑定事件
  PubSub.unsubscribe(this.token)
}
}
</script>

<style>
.card {
  float: left;
  width: 33.333%;
  padding: .75rem;
  margin-bottom: 2rem;
  border: 1px solid #efefef;
  text-align: center;
}

.card > img {
  margin-bottom: .75rem;
  border-radius: 100px;
}

.card-text {
  font-size: 85%;
}
</style>