<script setup lang="ts">
import type { Pokemon } from '~/app.vue';


interface Props {
    pokemon: Pokemon,
    isShown: boolean
}
const { pokemon, isShown } = defineProps<Props>()
</script>

<template>
    <li @click="$emit('showCard')"  class="card" :class="{ 'shown': isShown }">
            <div class="content">
                <div class="back">
                    <img src="assets/back.png" alt="" class="back-cover">
                </div>
                <div class="front">
                    <!-- Math.random for cache busting -->
                    <img :src="pokemon.sprites.front_default +  '?' + Math.random()" :alt="pokemon.name" />
                </div>
            </div>
    </li>
</template>
<style>

.card {
position: relative;
height: 100%;
list-style: none;
max-width: 10rem;
max-height: 13.875rem;
width: 100%;
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
    display: grid;
    align-items: center;
    justify-content: center;
    justify-items: center;
}
.back {
    cursor: pointer;
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