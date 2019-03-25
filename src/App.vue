<template>
  <div>
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
import Rules from './components/Rules.vue';
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
    gameRules: Rules,
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
    })
  }
}
</script>

<style scoped>

  .container{
    border: .5px solid black;
    width: 80vw;
    height: 90vh;
  }

</style>
