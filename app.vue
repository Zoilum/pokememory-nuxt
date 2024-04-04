<script setup lang="ts">
import { constNull, identity, pipe } from "fp-ts/function";
import * as t from "io-ts";
import * as E from "fp-ts/Either";
import * as RA from 'fp-ts/ReadonlyArray'
const MIN_POKEMONS_QTY = 4;
const FIRST_GEN_POKEMONS_COUNT = 151;

export interface Pokemon {
  name: string;
  sprites: { front_default: string }
}

const PokemonsCodec = t.array(t.type({
  name: t.string, sprites: t.type({ front_default: t.string })
}));
const showedCards = useState<string[]>('showedCards', () => [])
const pairedCards = useState<string[][]>('pairedCards', () => [])
// Fetch random pokemons
const { data, status } = await useAsyncData("pokemons", async () => {
  const pokemons = await Promise.all([
    $fetch(
      `https://pokeapi.co/api/v2/pokemon/${getRandomInt(
        FIRST_GEN_POKEMONS_COUNT
      )}`
    ),
    $fetch(
      `https://pokeapi.co/api/v2/pokemon/${getRandomInt(
        FIRST_GEN_POKEMONS_COUNT
      )}`
    ),
    $fetch(
      `https://pokeapi.co/api/v2/pokemon/${getRandomInt(
        FIRST_GEN_POKEMONS_COUNT
      )}`
    ),
    $fetch(
      `https://pokeapi.co/api/v2/pokemon/${getRandomInt(
        FIRST_GEN_POKEMONS_COUNT
      )}`
    ),
  ]);
  return pokemons;
});


// Validate fetch pokemons response
const pokemons = computed(() =>
  pipe(
    data.value,
    PokemonsCodec.decode,
    E.match(constNull, (pokemons) =>
      [...pokemons, ...pokemons].sort(() => 0.5 - Math.random())
    )
  )
);
console.log(pokemons.value)
const handleShowCard = (cardId: string) => {
  if (showedCards.value.length === 2 ||
    cardId === showedCards.value[0] ||
    pairedCards.value.some(pair => pair.includes(cardId))) return
  if (showedCards.value.length === 1) {
    showedCards.value = [showedCards.value[0], cardId]
    return
  }
  showedCards.value = [cardId]
}
watch(showedCards, async cards => {
  debugger;
  if (!cards[0] || !cards[1]) return
  if (cards[0].slice(0, -2) === cards[1].slice(0, -2)) {
    pairedCards.value = [...pairedCards.value, [cards[0], cards[1]]]
    showedCards.value = []
  } else {
    await new Promise(() => setTimeout(() => showedCards.value = [], 1000))
  }
})
watch(pairedCards, async cards => {
  if (RA.flatten(pairedCards.value).length !== pokemons.value?.length) return
  await new Promise(() => setTimeout(() => pairedCards.value = [], 2000))
})
</script>

<template>
  <ul v-if="pokemons" class="game-grid">
    <Card v-for="pokemon, index in pokemons" :pokemon="pokemon"
      @show-card="() => handleShowCard(`${pokemon.name}-${index}`)"
      :isShown="showedCards.includes(`${pokemon.name}-${index}`) || pairedCards.some(pair => pair.includes(`${pokemon.name}-${index}`))">
    </Card>
  </ul>
  <div v-else>
    <Loader :status="status"></Loader>
  </div>
</template>

<style>
html,
body,
ul {
  margin: 0;
  padding: 0;
  width: 100dvw;
  height: 100dvh;
}

li {
  list-style: none;
}

.game-grid {
  max-width: 100%;
  max-height: 100%;
  display: grid;
  justify-content: center;
  grid-template-rows: 6fr 6fr;
  grid-template-columns: 3fr 3fr 3fr 3fr;
}
</style>
