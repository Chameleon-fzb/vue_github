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
 getUsers(value){
    this.info={...this.info,...value}
 }
}
,
mounted(){
  this.$bus.$on('get-data',this.getUsers)
},
beforeDestroy(){
  this.$bus.$off('get-data')
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