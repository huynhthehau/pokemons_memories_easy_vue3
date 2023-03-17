<template>
    <div class="card" :class="{disabled: isDisabled}"
    :style="{height:`${heightCard}px`,
    width:`${(heightCard*3)/4}px`,
    perpective:`${(heightCard*3)/4*2}px`
    }">
        <div class="card__inner" :class="{'is-flipped':isFlipped}"
        @click="onToggleFlippedCard">
            <div class="card__face card_face--front" >
                <div class="card__content" :style="{backgroundSize:`${(heightCard*3)/4/3}px
                 ${(heightCard*3)/4/3}px`}"></div>
            </div>
            <div class="card__face card_face--back">
                <div class="card__content" :style="{ backgroundImage: 'url(./src/assets/'+ imgBackFaceUrl +')'}"></div>
            </div>
        </div>

    </div>
</template>

<script >
export default{props:{

imgBackFaceUrl:{
    type:String,
    require:true
},
cardsContext:{
    type:Array,
    default:function(){
        return []
    }
},
card:{
        type:[Object,String,Array,Number]
}},
    data(){
        return{
            isDisabled:false,
            isFlipped:false,
            heightCard:(920-16*4)/Math.sqrt(this.cardsContext.length)-16
        }
    } ,methods:{
        onToggleFlippedCard(){
            if(this.isDisabled) return;
            this.isFlipped=!this.isFlipped
            if(this.isFlipped) this.$emit("onFlip",this.card)
        },
        onFlipBackCard(){
            this.isFlipped=false
        },
        turnOnDisabled(){
            this.isDisabled =true;
        }
    },
}
</script>

<style lang="css" scoped>
    .card{
        display: inline-block;
        margin-right: 1rem;
        margin-bottom: 1rem;
    }
    .card.disabled .card__inner{
        cursor:default;
    }
    .card__inner{
        width: 100%;
        height: 100%;
        transition: transform 1s;
        transform-style: preserve-3d;
        cursor: pointer;
        position: relative;
    }
    .card__inner.is-flipped{
        transform: rotateY(-180deg);
    }

    .card__face{
        position: absolute;
        width: 100%;
        height: 100%;
        backface-visibility: hidden;
        overflow: hidden;
        border-radius: 1rem;
        padding:1rem;
        box-shadow: 0 3px 10px 3px rgba(0,0,0,0.2);
    }
    .card_face--front .card__content{
        background: url(../assets/images/icon_back.png) no-repeat center center;
        height: 100%;
        width: 100%;
    }
    .card_face--back{
        background-color: var(--light);
        transform: rotateY(-180deg);
    }
    .card_face--back .card__content{
        background-size: contain;
        background-position: center center;
        background-repeat: no-repeat;
        height: 100%;
        width: 100%;
    }
</style>