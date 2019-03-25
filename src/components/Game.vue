<template>
    <div>
        <button @click="goBack">Go Back</button>
        <h1>Game Page</h1>
        <p>{{userName}}</p>
        <p>{{userAvatar}}</p>
        
    </div>
</template>

<script>
import { eventBus } from '../main';

export default {
    data:function(){
        return{
            goingBack: 'firstPage'
        }
    },
    props:['userName',
            'userAvatar'],
    methods:{
        goBack(){
            eventBus.$emit("goingBack", this.goingBack);
        }
    }

}
</script>

<style>

</style>
