<template>
  <div>
    <link href="https://fonts.googleapis.com/css?family=Press+Start+2P" rel="stylesheet">
    <div class="container">
      <component :is="selectedComp" :userName='userName' :userAvatar='userAvatar'>

      </component>
    </div>
    <br>
    <!-- <audio controls src="\src\assets\come.mp3" autoplay="true" loop></audio>
    -->
  </div>
  
</template>

<script>
import firstPage from './components/FirstPage.vue';
import gameRules from './components/Rules.vue';
import GameAction from './components/Game.vue';
import {eventBus} from './main';

export default {
  data: function(){
    return{
      selectedComp: "firstPage",
      userName: '',
      userAvatar: ''
    }
  },

  components:{
    firstPage: firstPage,
    gameRules: gameRules,
    gameAction: GameAction,
  },

  created(){
    eventBus.$on("gameStarting",(incomingArray)=>{
      this.userName = incomingArray[0];
      this.userAvatar = incomingArray[1];
      this.selectedComp = incomingArray[2];
    });
    eventBus.$on("goingBack", (comp)=>{
      this.selectedComp = comp;
    });
    eventBus.$on("showRules", (comp)=>{
      this.selectedComp = comp;
    });
    eventBus.$on("backToTheGame", (comp)=>{
      this.selectedComp = comp;
    });
  }
}
</script>

<style scoped>
   body{
    width: 95vw;
    height: 95vh;
    text-align: center;
    background: black;
    font-family: 'Press Start 2P';
  }

  .container{
    margin: auto;
    border: .5px solid black;
    width: 100%;
    height: 100vh;
    min-width: 500px;
    text-align: center;
    background: black;
    color: bisque;
  }


</style>
