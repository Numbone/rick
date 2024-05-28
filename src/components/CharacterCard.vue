<template>
  <div class="character-card">
    <div class="character_image">
      <img :src="character.image" alt="character.name" />
    </div>
    <div class="character_content">
      <section class="section">
        <div>
          <h2>{{ character.name }}</h2>
        </div>
        <span class="status">
          <div class="status__icon" :style="{backgroundColor:handleStatusColor(character.status)}"></div>
          {{ character.status }}-{{ character.species }}
        </span>
      </section>
      <section class="section">
        <div class="text-gray">
          Last known location:
        </div>
        <div>
          <div class="text">{{ character.location.name }}</div>
        </div>
      </section>
      <section class="section">
        <div class="text-gray">
          First seen in:
        </div>
        <div>
          <div class="text">{{ character.location.name }}</div>
        </div>
      </section>
    </div>

  </div>
</template>

<script setup lang="ts">
import { Character } from '../types/character';

const handleStatusColor=(color:string)=>{
  switch (color) {
    case 'Alive':
      return 'green';
    case 'Dead':
      return 'red';
    default:
      return 'gray';
  }
}

defineProps<{
  character: Character;
}>();
</script>

<style scoped>
.character-card {
  width: 600px;
  height: 220px;
  display: flex;
  overflow: hidden;
  background: rgb(60, 62, 68);
  border-radius: 0.5rem;
  margin: 0.75rem;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px, rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
}

.character-card img {
  width: 100%;
  height: 100%;
  margin: 0px;
  opacity: 1;
  transition: opacity 0.5s ease 0s;
  object-position: center center;
  object-fit: cover;
}

.character_image {
  flex: 2 1 0%;
  width: 100%;
}

.character_content {
  flex: 3 1 0%;
  position: relative;
  padding: 0.75rem;
  color: rgb(255, 255, 255);
  display: flex;
  flex-direction: column;
}

.section {
  flex: 1 1 0%;
  display: flex;
  flex-direction: column;
}

.text-gray {
    color: rgb(158, 158, 158);
}

.status {
    display: flex;
    -webkit-box-align: center;
    align-items: center;
    text-transform: capitalize;
}

.status__icon {
    height: 0.5rem;
    width: 0.5rem;
    margin-right: 0.375rem;
    border-radius: 50%;
}
</style>
