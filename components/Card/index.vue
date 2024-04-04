<script setup lang="ts">
import type { Pokemon } from '~/app.vue';


interface Props {
    pokemon: Pokemon,
    isShown: boolean
}

const { pokemon, isShown } = defineProps<Props>()

const pokemonImg = ref(pokemon.sprites.front_default)

const image = computed(() =>  pokemonImg.value)
</script>

<template>
    <li @click="$emit('showCard')" >
        <div class="card" :class="{ 'shown': isShown }">
            <div class="content">
                <div class="back">
                    <img src="assets/back.png" alt="" class="back-cover">
                </div>
                <div class="front">
                    <img :src="image" :alt="pokemon.name" />
                   <h1 style="background: red;">
                    {{ pokemon.name }}
                   </h1>
                </div>
            </div>
        </div>
    </li>
</template>
<style>

.card {
position: relative;
}

.content {
    position: absolute;
    width: 100%;
    height: 100%;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
    transition: transform 1s;
    transform-style: preserve-3d;
}

.card.shown .content {
    transform: rotateY(180deg);
    transition: transform 0.5s;
}

.front,
.back {
    position: absolute;
    height: 100%;
    width: 100%;
    color: #03446A;
    text-align: center;
    backface-visibility: hidden;
}

.front {
    background: red;
    color: white;
    transform: rotateY(180deg);
}
.back-cover {
    display: block;
    max-width: 100%;
}
</style>