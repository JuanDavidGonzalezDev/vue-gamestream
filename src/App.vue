<script setup>
import LayoutHero from './components/Layout/LayoutHero.vue'
import {onMounted, reactive, ref} from 'vue';
import GameCard from './components/Games/GameCard.vue';
import GameLayout from './components/Games/GameLayout.vue';
import GameModal from './components/Games/GameModal.vue';

const API_URL = 'https://gamestreamapi.herokuapp.com/api/games'

const gamesView = ref([]);
const fetchGames = async () => {
  try{
    state.isLoading = true
    const response = await fetch(API_URL)
    const  json = await response.json()
    state.data = json
    gamesView.value = json;

  }catch(error){
    console.error(error)
    state.error = error
  }finally{
    state.isLoading = false
  }
}

const state = reactive ({
  error: null,
  isLoading: false,
  data: []
});

onMounted(()=>{
  fetchGames();
})

const setGameView = (filteredGames) => {
  gamesView.value = filteredGames
}

</script>

<template>
  <LayoutHero />
  <main>
      <GameLayout :games="state.data" @set-game-view="setGameView">
      <template #title>
        <h3>Juegos actualizados</h3>
      </template>
      <GameCard v-for="game in gamesView" :key="game.title" :game="game"/>
    </GameLayout>
    <Teleport to="body">
      <GameModal/>
    </Teleport>
  </main>
</template>

<style scoped>
main {
  padding: 2rem;
}
</style>
