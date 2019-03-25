<template>
  <div>
    <h1>Landing Page</h1>

    <img class='avatar image1' @click="userInformation[1] = 1">1
    <img class='avatar image2' @click="userInformation[1] = 2">2
    <img class='avatar image3' @click="userInformation[1] = 3">3
    <img class='avatar image4' @click="userInformation[1] = 4">4
    <br>
    <input type="name" value="name" v-model="userInformation[0]">
    <br>
    
    <div>
      <p> {{userInformation[0]}} </p>
      <p> {{userInformation[1]}} </p>
    </div>
    <br>
    <button @click="startGame"> Start the Game </button>
    
    
  </div>
</template>

<script>
import { eventBus } from '../main';

export default {
  data: function(){
    return{
      userInfo: false,
      userInformation: ["","","gameAction"],
      imgList: ["/src/assets/images/char1.png"]

    }
  },
  methods:{
    startGame(){
      eventBus.$emit("gameStarting", this.userInformation);
    }
  }

}
</script>

<style>
.avatar{
  height: 130px;
  width: 130px;
}
</style>
