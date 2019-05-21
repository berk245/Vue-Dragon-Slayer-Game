<template>
  <div class="container">
    <img class= "dragon dragon1" src="../assets/images/dragon.png" alt="">
    <img class= "dragon dragon2" src="../assets/images/dragon.png" alt="">

    <div class="flex">
      <img class="sword" src="../assets/images/sword.png" alt="sword">
      <h1 class="gameTitle">DRAGON SLAYER</h1>
      <h2> How Does Our Hero Look Like?</h2>
      <span class="avatars">
        <img @click="avatarPick1" src= "/src/assets/images/char1.png" class='avatar image1' :class="{'avatarClick' : clicked1}">
        <img @click="avatarPick2" src= "/src/assets/images/char2.png" class='avatar image2' :class="{'avatarClick' : clicked2}">
        <img @click="avatarPick3" src= "/src/assets/images/char3.png" class='avatar image3' :class="{'avatarClick' : clicked3}">
        <img @click="avatarPick4" src= "/src/assets/images/char4.png" class='avatar image4' :class="{'avatarClick' : clicked4}">
      </span>
      <input  @keyup.enter="startGame" class="input" type="name" value="name" v-model="userInformation[0]" placeholder="The name of our hero?...">
      <button class="startButton" @click="startGame"> Start the Game </button>
    </div>  
  </div>
</template>

<script>
import { eventBus } from '../main';

export default {
  data: function(){
    return{
      userInfo: false,
      userInformation: ["","","gameRules"],
      clicked1: false,
      clicked2: false,
      clicked3: false,
      clicked4: false,
    }
  },
  methods:{
    startGame(){
      eventBus.$emit("gameStarting", this.userInformation);
    },

    avatarPick1(){
      this.clicked2 = false;
      this.clicked3 = false;
      this.clicked4 = false;
      this.clicked1 = !this.clicked1;
      this.userInformation[1] = 1;
    },
    avatarPick2(){
      this.clicked1 = false;
      this.clicked3 = false;
      this.clicked4 = false;
      this.clicked2 = !this.clicked2;
      this.userInformation[1] = 2;
    },
    avatarPick3(){
      this.clicked1 = false;
      this.clicked2 = false;
      this.clicked4 = false;
      this.clicked3 = !this.clicked3;
      this.userInformation[1] = 3;
    },
    avatarPick4(){
      this.clicked2 = false;
      this.clicked3 = false;
      this.clicked1 = false;
      this.clicked4 = !this.clicked4;
      this.userInformation[1] = 4;
    },


  }

}
</script>

<style scoped>
@keyframes welcome {
  0%{
    border: solid 4px red;
  }
  25%{
    border: solid 4px purple;
  }
  50%{
    border: solid 4px blue;
  }
  75%{
    border: solid 4px green;
  }
  100%{
    border: solid 4px yellow;
  }

}
@keyframes swordMove {

  50%{
    transform:scaleX(-1);
  }
}
@keyframes dragonMove {
  0%{ transform: translateY(0%); }
  50%{ transform: translateY(80%); }
  100%{ transform: translateY(0%); }

}
html{
  background-color: black;
}

.container{
  width: 100vw;
  min-width: 700px;
  height: 120vh;
  border: solid 1px red;
  display: grid;
  grid-template-columns: 30% 40% 30%;
  grid-template-rows: 25% 50% 25%;
  /*  dragon main1-emp dragon
      dragon kılınç    dragon
      dragon name      dragon
      emp    avatars   emp
      emp    input     emp
      emp    start     emp
      emp    emp       emp
          */
}
.dragon{
  width: 100%;
}
.dragon1{
  grid-column: 1 / span 1;
  animation-name: dragonMove;
  animation-duration: 5s;
  animation-fill-mode: both;
  animation-iteration-count: infinite;
}
.dragon2{
  grid-column: 3 / span 1;
  animation-name: dragonMove;
  animation-duration: 5s;
  animation-fill-mode: both;
  animation-iteration-count: infinite;
}
.flex{
  text-align: center;
  grid-row: 1 / span 2;
  grid-column: 2 / span 1;
  display: flex;
  flex-direction: column;
}
.sword{
  animation-name: swordMove;
  animation-duration: 5s;
  animation-iteration-count: infinite;
  width:100%;
}
.gameTitle{
  animation-name: welcome;
  animation-duration: 5s;
  animation-iteration-count: infinite;
  margin:auto;
  margin-bottom: 10%;
  width: 100%;

}
.avatars{
    display: flex;
    margin-bottom: 15%;
}
.avatar{
  height: 100%;
  width: 25%;
  min-height:70px;
  min-width: 70px;
}
.avatarClick{
  height: 100%;
  width: 25%;
  min-height:70px;
  min-width: 70px;
  border: 1px solid red;
}
.input{
  font-size: 25px;
  margin-top: 10px;
  width: 70%;
  align-self: center;
  text-align: center;

}
.startButton{
  margin-top: 10px;
  width: 50%;
  align-self: center;
  background-color: black;
  color: red;
}

</style>
