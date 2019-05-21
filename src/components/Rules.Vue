<template>
    <div class="ruleBox">
        <p>
              Dragon is Strong! Are You Smart Enough to Take it Down? 
              <br><br><br>
              You Attack First! Use This to Your Advantage!
              <br><br><br>
              Every Round Dragon Automatically Attacks You Back, Check the Damage You have Received from the List Below. <br><br><br>
              For Every Attack You Make, You Get a Chance to Heal. But Monster May Not Be So Kind To Wait!<br><br><br>
              5 Successful Attacks Earn You a Special Attack. Every Heal You Do Will Decrement the Special Attack Counter! Use it Wisely!<br><br><br>
            </p>
        <button @click="backToTheGame"> Got It, things couldn't be more clear!</button>
    </div>
</template>

<script>
import { eventBus  } from "../main";
export default {
    data:function(){
        return{
            backToGame: 'gameAction',
        }
    },
    methods:{
        backToTheGame(){
            eventBus.$emit("backToTheGame", this.backToGame);
        }
    }

}
</script>

<style>
.ruleBox{
    height: 100vh;
}

</style>
