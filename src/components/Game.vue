<template>
    <div>
        <p>{{ avatars }}</p>
        <button @click="showRules = !showRules">Rules</button>
        <game-rules v-show="showRules"></game-rules>
        <button @click="passData"></button>
    </div>
</template>

<script>
import {eventBus} from "../main";
import gameRules from './Rules.vue';

export default {
    data: function(){
        return{
            avatars:'belki',
            showRules: false,
            passingData: "firstPage"
        }
    },
    components:{
        gameRules: gameRules,
    },
    created(){
        eventBus.$on("avatarSet", (avatar)=>{
            this.avatars = avatar;
    })
    }

}
</script>

<style>
</style>
