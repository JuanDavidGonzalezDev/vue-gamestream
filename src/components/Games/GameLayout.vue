<script setup>
import { useSlots, ref } from 'vue';
import SharedSearch from '../Shared/SharedSearch.vue';

const slots = useSlots();
const searchInput  = ref("")

const emit = defineEmits(['setGameView'])

const {games} = defineProps({
  games:{
    type: Array,
    required: true
  }
})

const onSearch = ()=>{
  const termSearch = searchInput.value.toLowerCase()
  
  if(termSearch.trim() ===''){
    emit('setGameView', games)
  }

  const filteredGames = games.filter((game)=>{
    return (game.title.toLowerCase().includes(termSearch))
  })
  emit('setGameView', filteredGames)
}

</script>

<template>
  <section>
    <slot name="title"/>
    <h2 v-if="slots.title == undefined">Recent games</h2>
    {{ searchInput }}
    <div class="game-layout">
      <SharedSearch @search="onSearch" v-model="searchInput"/>
      <slot/>
    </div>
  </section>
</template>

<style scoped>
.game-layout {
  display: grid;
  gap: 2rem;
  margin: 1rem auto;
  max-width: 90%;
}
</style>
