<template>
    <div>
        <div id="pokemon_imagem">
            <figure>
                <img  class="center" :src="currentImg" alt="Placeholder image">
            </figure>
        </div>
        <h1 class="pokemon_data">
            <span class="pokemon_name"> {{upper(name)}} </span> -
            <span> {{ pokemon.type}}</span>
        </h1>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    created: function(){
        axios.get(this.url).then(res => {
            this.pokemon.type = res.data.types[0].type.name;
            this.pokemon.front = res.data.sprites.front_default;
            this.pokemon.back = res.data.sprites.back_default;
            this.currentImg = this.pokemon.front;
        })
    },
    data(){
        return{
            isFront: true,
            currentImg: '',
            pokemon: {
                type: '',
                front: '',
                back: ''
            }
        }
    },
    props: {
        num: Number,
        name: String,
        url: String
    },
    methods:{
        upper: function(value){
            var newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        },
        mudarSprite: function(){
            if(this.isFront){
                this.isFront = false;
                this.currentImg = this.pokemon.back;   
            }else{
                this.isFront = true;
                this.currentImg = this.pokemon.front;
            }
        }
    }
}
</script>

<style>

#pokemon_imagem{
    position: absolute;
    bottom: 54%;
    left: 48%;
    transform: translate(-63%, 20%);
    height: 18%;
}

.pokemon_data {
    position: absolute;
    font-weight: 650;
    color: #aaa;
    right: 34%;
    top: 54%;
    font-size: clamp(8px, 5vw, 25px);
}

.pokemon_name {
    color: #3a444d;
    text-transform: capitalize;
}
</style>