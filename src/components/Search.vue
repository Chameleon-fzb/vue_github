<template>
  <section class="jumbotron row">
      <h3 class="jumbotron-heading">Search Github Users</h3>
      <div>
        <input type="text" v-model='keyWord' placeholder="enter the name you search" @keyup.enter="searchUser"/>
        <button @click="searchUser" >Search</button>
      </div>
    </section>
</template>

<script>
import PubSub from 'pubsub-js'
import axios from 'axios'
export default {
 name:"Search",
 data(){
   return{
     keyWord:''
   }
 },
 methods:{
  async  searchUser(){
    // this.$bus.$emit('get-data',{isFirst:false,isLoading:true})
    PubSub.publish('get-data',{isFirst:false,isLoading:true});
    try {
      const res = await axios.get('https://api.github.com/search/users',{params:{
      q:this.keyWord
    }})
    const {items} = res.data
    //  this.$bus.$emit('get-data',{isLoading:false,users:items})
     PubSub.publish('get-data',{isLoading:false,users:items})
    } catch (error) {
        // this.$bus.$emit('get-data',{isLoading:false,users:[],errMsg:error.message})
        PubSub.publish('get-data',{isLoading:false,users:[],errMsg:error.message})
    }
    
   }
 }
}
</script>