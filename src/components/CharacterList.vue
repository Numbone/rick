<template>
  <div class="filters">
    <input v-model="name" placeholder="Name" />
    <select v-model="status">
      <option value="">All</option>
      <option value="alive">Alive</option>
      <option value="dead">Dead</option>
      <option value="unknown">Unknown</option>
    </select>
    <button @click="applyFilters">Apply</button>
  </div>
  <div class="characters">
    <CharacterCard v-for="character in characters" :key="character.id" :character="character" />
  </div>
  <div class="pagination">
    <button @click="prevPage" :disabled="page === 1">Previous</button>
    <span class="page">Page {{ page }}</span>
    <button @click="nextPage">Next</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue';
import axios from 'axios';
import CharacterCard from './CharacterCard.vue';
import { Character } from '../types/character';



export default defineComponent({
  name: 'CharacterList',
  components: {
    CharacterCard
  },
  setup() {
    const characters = ref<Character[]>([]);
    const page = ref(1);
    const name = ref('');
    const status = ref('');

    const fetchCharacters = async () => {
      const response = await axios.get('https://rickandmortyapi.com/api/character', {
        params: {
          page: page.value,
          name: name.value,
          status: status.value
        }
      });
      characters.value = response.data.results;
    };

    const applyFilters = () => {
      page.value = 1
    };

    const prevPage = () => {
      if (page.value > 1) {
        page.value -= 1;
      }
    };

    const nextPage = () => {
      page.value += 1
    };

    watch([page, name, status], fetchCharacters, { immediate: true });

    return { characters, page, name, status, applyFilters, prevPage, nextPage };
  }
});
</script>

<style scoped>
.characters {
  display: flex;
  -webkit-box-pack: center;
  justify-content: center;
  -webkit-box-align: center;
  align-items: center;
  flex-wrap: wrap;
  max-width: 1920px;
  box-shadow: 0 0 0 100vmax rgb(39, 43, 51);
  clip-path: inset(0-100vmax);
  background-color: rgb(39, 43, 51);
}

.filters {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}

.filters input,
.filters select,
.filters button {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.filters button {
  background-color: #007bff;
  color: white;
  cursor: pointer;
}

.filters button:hover {
  background-color: #0056b3;
}

.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin-top: 20px;
}

.pagination button {
  padding: 10px 20px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
}

.pagination button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.pagination button:hover:enabled {
  background-color: #0056b3;
}

.pagination span {
  font-size: 18px;
}

.page {
  color: rgb(39, 43, 51)
}
</style>