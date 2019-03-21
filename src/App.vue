<template>
  <div>
    <div class="container">
      <component :is="selectedComp">

      </component>
      <p>{{selectedComp}}</p>
      <p>{{avatars}}</p>
    </div>
    <br>
    <audio controls src="\src\assets\come.mp3" autoplay="true" loop></audio>
  </div>
  
</template>

<script>
import firstPage from './components/First-Page.vue';
import Rules from './components/Rules.vue';
import GameAction from './components/Game.vue';
import {eventBus} from './main';

export default {
  data: function(){
    return{
      selectedComp: "firstPage",
      avatars: ''
    }
  },

  components:{
    firstPage: firstPage,
    gameRules: Rules,
    gameAction: GameAction,
  },

  created(){
    eventBus.$on("gameStarting",(newComponent)=>{
      this.selectedComp = newComponent;
    });
    eventBus.$on("avatarSet", (avatar)=>{
      this.avatars = avatar;
    });
    eventBus.$on("passingBy", (comp)=>{
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
