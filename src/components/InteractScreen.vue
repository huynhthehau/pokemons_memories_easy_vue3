<template>
   <div class="screen">
    <div class="screen__inner" :style="{width:`${widthScreen}px`}">
        <card-flip :cardsContext="cardsContext" v-for="(card,index) in cardsContext" :key="index" :ref='`card-${index}`'
    :imgBackFaceUrl="`images/${card}.png`" :card="{index:index,value:card}" @onFlip="checkRule($event)"/>
    </div>
   </div>
</template>
<script>
import CardFlip from "./Card.vue"

export default {
    props:{
        cardsContext:{
            type:Array,
            default: function(){
                return [];
            }
        }
    },
    components:{
        CardFlip,
    },
    data(){
        return{
            rules:[],
            totalRight:0,
            widthScreen:((((920-16*4)/Math.sqrt(this.cardsContext.length)-16)*3)/4 +16 )* Math.sqrt(this.cardsContext.length)
        }
    },
    methods:{
        checkRule(card){
            if(this.rules.length===2) return false;
            this.rules.push(card)
            if(this.rules.length===2&& this.rules[0].value ===this.rules[1].value){
                this.$refs[`card-${this.rules[0].index}`][0].turnOnDisabled()
                this.$refs[`card-${this.rules[1].index}`][0].turnOnDisabled()
                this.rules=[]
                this.totalRight++
                if(this.totalRight*2==this.cardsContext.length){
                    setTimeout(()=>{
                        this.$emit('onFinish')
                    },2000)
                }
            }else if(this.rules.length===2&&this.rules[0].value!==this.rules[1].value){
                const ref1=`card-${this.rules[0].index}`
                const ref2=`card-${this.rules[1].index}`
                //close two card
                setTimeout(()=>{
                        this.$refs[ref1][0].onFlipBackCard()
                        this.$refs[ref2][0].onFlipBackCard()
                    },800)
                    this.rules=[]
            }
        }
    }
}
</script>
<style lang="css" scoped>
    .screen{
        width:  100%;
        height: 100vh;
        position: absolute;
        top: 0;
        left: 0;
        z-index: 2;
        background-color: var(--dark);
        color:var(--light)
    }
    .screen__inner{
        display: flex;
        flex-wrap: wrap;
        margin: 2rem auto;
    }
</style>

