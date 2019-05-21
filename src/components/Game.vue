<template>
    <div class="gameContainer">
        <div>
            <button id="rulesButton" @click="showGameRules" style="width: 50px"> Rules </button>
        </div>
        <div class="gameBox">
            <div class="userBox">
                <img :src="pic_url" alt="user Avatar" class="userAvatar">
                <p>{{userName}}</p>
                <div class="bar" v-bind:style='{ width: playerHealth + "%"}'>
                    <span>{{playerHealth}}</span>
                </div>
                <div class=" bar special" v-bind:class="{ spReady : attackCounter >= 5 }" 
                                          v-bind:style=' {width: attackCounter * 20 + "%", "max-width": 100 + "%"}'> Special <span v-if="attackCounter >= 5"> Ready</span> </div>
            </div>
            <div class="dragonBox">
                <img src="src\assets\images\dragon2.png" alt="dragon-picture">
                <p>Dragon</p>
                <div class="bar" v-bind:style='{ width: dragonHealth  + "%"}'>
                    <span>{{dragonHealth}}</span>
                </div>
                 <div class="bar special" style="opacity: 0;" ></div>
            </div>
        </div>
        <div class="buttons">
            <button id="normalButton" @click="normalAttack"> Attack </button>
            <button id="specialButton" @click="specialAttack"> Special Attacks</button>
            <button id="healButton" @click="healUp"> Heal </button>
            <button id="giveUpButton"> Give Up </button>
        </div>
        <div class="specials" v-if="specialReady && attackCounter >= 5">
            <button id="mightyButton" @click="mightyAttack"> Mighty Attack </button>
            <button id="superHealButton" @click="superHeal"> Super Heal </button>
            <button id="confuseButton" @click="confuseAttack"> Confusion Attack </button>
            <button> Phone-A-Friend</button>
        </div>
        <div v-if="gameOver"> <button @click="newGame"> Start Again </button></div>
        <div class="eventLog">
            <p v-for="line in log.slice(0, 6)"> {{ line }} </p>
            </div>
    </div>
</template>

<script>
import { eventBus } from '../main';
import rules from './Rules.vue';

export default {
    data:function(){
        return{
            goingBack: 'firstPage',
            pic_url: "/src/assets/images/char" + this.userAvatar + ".png",
            showRules: 'gameRules',
            roundWon: false,
            playerHealth: 100,
            dragonHealth: 100,
            attackCounter: 0,
            dragonAttackCounter: 0,
            specialReady: false,
            log: [],
            dragonDazed: false,
            dazedCount: 0,
            gameOver: false,
        }
    },
    props:['userName',
            'userAvatar'],
    components:{
        gameRules: rules
    },
    methods:{
        newGame(){
            this.gameOver = false;
            this.log = ["A new game begins"],
            this.dragonHealth = 100;
            this.dragonAttackCounter= 0;
            this.dragonDazed=false;
            this.playerHealth = 100;
            this.attackCounter = 0;
            this.specialReady = false;
        },
        goBack(){
            eventBus.$emit("goingBack", this.goingBack);
        },
        showGameRules(){
            if(!this.gameOn){
                eventBus.$emit("showRules", this.showRules);
            }
            else{
                alert("Check the rules before or after the game! Sorry");
            }
        },

        //PLAYER ATTACKS
        normalAttack(){
            if(this.dragonHealth > 0 && this.playerHealth > 0){
                this.dragonHealth -= 7;
                this.log.unshift(this.userName + " attacks bravely and does 7 damage");
                this.attackCounter++;
                if(this.dragonHealth <= 0){
                    this.log.unshift(this.userName + " takes down the dragon!");
                    alert(this.userName + "wins!");
                    this.gameOver = true;
                }
                else{
                    if(this.dazedCount == 0 && this.dragonDazed){
                        this.dazedCount++;
                        this.log.unshift("Dragon looks dazed");
                    }
                    else if(this.dazedCount == 1 && this.dragonDazed){
                        this.dazedCount++;
                        this.log.unshift("Dragon still looks dazed");
                        this.dragonDazed = false;
                    }
                    else{
                        this.dazedCount = 0;
                        this.dragonTurn();
                    }
                }
            }
        },
        specialAttack(){
            if(this.dragonHealth > 0 && this.playerHealth > 0 && this.attackCounter >= 5){
                this.specialReady = !this.specialReady;
            }

        },
        healUp(){
            if(this.dragonHealth > 0 && this.playerHealth > 0 && this.attackCounter >= 1){
                this.playerHealth += 10;
                this.log.unshift(this.userName + ' takes a moment and heals!');
                this.attackCounter--;
                if(this.dazedCount == 0 && this.dragonDazed){
                    this.dazedCount++;
                    this.log.unshift("Dragon looks dazed");
                }
                else if(this.dazedCount == 1 && this.dragonDazed){
                    this.dazedCount++;
                    this.log.unshift("Dragon still looks dazed");
                    this.dragonDazed = false;
                }
                else{
                    this.dazedCount = 0;
                    this.dragonTurn();
                }
            } 
        },
        mightyAttack(){
            this.dragonHealth -= 18;
            this.log.unshift("MIGHTY ATTACK. A HUGE DAMAGE DONE and A MORALE BOOST!!");
            this.attackCounter = 0;
            this.specialReady = false;
            if(this.dragonHealth <= 0){
                    this.log.unshift(this.userName + " takes down the dragon!");
                    alert(this.userName + "wins!");
                    this.gameOver = true;
                }
                else{
                    this.dragonTurn();
                    }
        },
        confuseAttack(){
            this.dragonDazed = true;
            this.specialReady = false;
            this.log.unshift("DRAGON LOOKS DAZED and CAN'T ATTACK YOU FOR 2 ROUNDS");
            this.attackCounter = 0;

        },

        superHeal(){
            this.playerHealth += 20;
            this.specialReady = false;
            this.log.unshift("SUCH A GOOD HEAL");
            this.attackCounter = 0;
            this.dragonTurn();
        },

        //DRAGON ATTACKS
        dragonNormal(){
            if(this.dragonHealth > 0 && this.playerHealth > 0){
                this.playerHealth -= 8;
                this.log.unshift("Dragon attacks and does 8 damage!");
                this.dragonAttackCounter++;
                if(this.playerHealth <= 0){
                     this.log.unshift("Dragon wins!");
                     alert("Dragon is victorious!");
                     this.gameOver = true;
                }
            }
        },
        dragonSpecial(){
            if(this.dragonHealth > 0 && this.playerHealth > 0){
                this.playerHealth -= 18;
                this.log.unshift("Dragon attacks with fury and does 18 damage!");
                this.dragonAttackCounter = 0;
                if(this.playerHealth <= 0){
                     this.log.unshift("Dragon wins!");
                     alert("Dragon is victorious!");
                     this.gameOver = true;
                }
            }
        },
        dragonHeal(){
            if(this.dragonHealth > 0 && this.playerHealth > 0 && this.dragonAttackCounter >= 1){
                this.dragonHealth += 10;
                this.dragonAttackCounter--;
                this.log.unshift("Dragon looks a bit refreshed!");
            }
            

        },
        //DRAGON LOGIC
        dragonTurn(){
            //FINISH HIM
            if(this.playerHealth <= 7){
                this.dragonNormal();
            }
            //CHECK IF THE USER IS ABOUT TO MAKE A SPECIAL ATTACK
            else if(this.attackCounter >= 4){
                if(this.dragonHealth < 15){
                    this.dragonHeal()
                }
                else{
                    if(this.dragonAttackCounter >= 4){
                    this.dragonSpecial();
                }
                 else{
                    this.dragonNormal();
                }
                }
            }
            //HEAL UP
            else if(this.dragonHealth < 8){
                this.dragonHeal();
            }
            //When everything is fine
            else{
                if(this.dragonAttackCounter >= 4){
                    this.dragonSpecial();
                }
                else{
                    this.dragonNormal();
                }
            }
            
        }
    }

    }
</script>

<style>
@keyframes ready {
  0%{
    border: solid 2px red;
  }
  25%{
    border: solid 2px yellow;
  }
  50%{
    border: solid 2px purple;
  }
  75%{
    border: solid 2px yellow;
  }
  100%{
    border: solid 2px purple;
  }

}

.gameContainer{
    text-align: center;
    justify-content: center;
    height: 100vh;
    width: 100vw;
    display: grid;
    /*
    EM PT Y
    EM 
    EM
    */
    grid-template-columns: 10% 80% 10%;
    grid-template-rows: 5% 35% 10% 10% 35% 5%;
}

.gameBox{
    width: 100%;
    text-align: center;
    grid-row: 2 / span 1;
    grid-column: 2 /span 1;
    display: grid;
    grid-template-columns: 10% 35% 10% 35% 10%;
}
.userBox{
    display: flex;
    flex-direction: column;
    justify-content: center;
    grid-column: 2 / span 1;
}
.dragonBox{
    display: flex;
    flex-direction: column;
    justify-content: center;
    grid-column: 4 / span 1;
}

.bar{
    
    height: 30px;
    border: .5px solid white;
    align-items: center;
    justify-self: right;
    background-color: green;
}
.special{
    height: 20px;
    background-color: red;
    color: yellow;
    border: 1px solid yellow;
}
.spReady{
    background-color: red;
    color: yellow;
    animation: ready;
    animation-duration: 3s;
    animation-direction: both;
    animation-iteration-count: infinite;
}

button{
    height: 60px;
    width: 20%;
    background-color: black;
    color: white;
    border: 2px solid gray;
    font-size: 15px;
}
#normalButton:hover{
    background-color: purple;
}
#healButton:hover{
    background-color: skyblue;
}
#giveUpButton:hover{
    background-color: white;
    color: black;
}
#specialButton:hover{
    background-color: yellow;
    color: red;
}
#mightyButton:hover{
    background-color: rgb(150, 12, 62);
}
#superHealButton:hover{
    background-color: rgb(8, 164, 153);
}
#confuseButton:hover{
    background-color: rgb(132, 94, 132);
}

.buttons{
    grid-row: 3 / span 1;
    grid-column: 2 / span 1;
}
.specials{
    grid-row: 4 / span 1;
    grid-column: 2 / span 1;
}

.eventLog{
    grid-column: 2 / span 1;
    grid-row: 5 / span 1;
    width: 80%;
    border: 1px solid red;
    justify-self: center;
}

img{
    width:130px;
    height:130px;
    background: black;
    margin-left: 30%;
}

</style>
