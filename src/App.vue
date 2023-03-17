<template>
  <main-screen v-if="statusMatch ==='default'" @onStart="(onHandleBeforeStart($event))"></main-screen>
  <interact-screen v-if="statusMatch ==='match'" :cardsContext="settings.cardsContext" @onFinish="onGetResult" ></interact-screen>
  <result-screen v-if="statusMatch==='result'" :timer="timer" @onStartAgain="statusMatch = 'default'"></result-screen>
  <copy-right/>
</template>
<script>
import MainScreen from "./components/MainScreen.vue"
import InteractScreen from "./components/InteractScreen.vue"
import ResultScreen from "./components/ResultScreen.vue"
import CopyRight from './components/CopyRight.vue'
import {shuffled} from './utils/array'
export default{
  name:"App",
  components:{
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight
  },
  data(){
    return{
      settings:{
        totalOfBlocks:0,
        cardsContext:[1,2,3,4],
        startedAt:0,
      },
      statusMatch:"default",
      timer:0
    }
  },methods:{
    onHandleBeforeStart(config){
       this.settings.totalOfBlocks = config.totalOfBlocks
      const firstCard = Array.from({length:this.settings.totalOfBlocks/2},(e, i)=>{
        return i+1
      })
      const secondCard = [...firstCard]
      const cards = [...firstCard,...secondCard]
      this.settings.cardsContext  = shuffled(shuffled(shuffled(cards)))
      this.settings.startedAt = new Date().getTime();
      this.statusMatch='match'
    },
    onGetResult(){
      this.timer = new Date().getTime() - this.settings.startedAt - 2000
      this.statusMatch ="result"
    }
  }
}
</script>