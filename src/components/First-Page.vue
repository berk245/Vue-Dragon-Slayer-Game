<template>
    <div>
        <p>This is the opening page</p>
        <p> {{userName}} </p>
        <section class="charImg">
            <button @click="chooseAvatar1" class="avatars avatar1"></button>
            <button @click="chooseAvatar1" class="avatars avatar2"></button>
            <button @click="chooseAvatar1" class="avatars avatar3"></button>
            <button @click="chooseAvatar1" class="avatars avatar4"></button>
          </section>

        <button @click="startGame">Comp Change</button>


    </div>
</template>

<script>
import {eventBus} from "../main";

export default {
    data: function(){
        return{
            userName: '',
            chosenAvatar: '',
            gameStart: "gameAction",
        }
    },
    methods:{
        chooseAvatar1(){
            this.chosenAvatar = "üstümüzden"; 
        },
        sendAvatar(){
            eventBus.$emit("avatarSet", this.chosenAvatar)
        },
        startGame(){
            eventBus.$emit("gameStarting", this.gameStart);
            this.sendAvatar();
        }
    },
    created(){
        eventBus.$on("passingBy",(dataPass)=>{
            this.userName = dataPass;
        })
    }

}
</script>

<style>
.avatars{
  width: 130px;
  height: 130px;
}

.avatar1{
  background-image: url('/src/assets/images/char1.png');
  background-repeat: no-repeat;
  background-size: contain;
}
.avatar2{
  background-image: url('/src/assets/images/char2.png');  
  background-repeat: no-repeat;
  background-size: contain;
}
.avatar3{
  background-image: url('/src/assets/images/char3.png');  
  background-repeat: no-repeat;
  background-size: contain;
}
.avatar4{
  background-image: url('/src/assets/images/char4.png');
  background-repeat: no-repeat;
  background-size: contain;
}


</style>
