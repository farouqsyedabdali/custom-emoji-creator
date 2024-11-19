<script setup>
import { ref } from 'vue';
import CharacterMenu from './CharacterMenu.vue';
import defaultFace from '../assets/defaultface.svg';

// Separate refs for each component
const selectedFace = ref(null);
const selectedEyes = ref(null);
const selectedMouth = ref(null);

const activeMenu = ref('faces');

const handleSelection = (option) => {
  if (option.type === 'face') {
    selectedFace.value = option;
  } else if (option.type === 'eyes') {
    selectedEyes.value = option;
  } else if (option.type === 'mouth') {
    selectedMouth.value = option;
  }
};
</script>

<template>
  <div class="grid grid-cols-2 gap-4 container mx-auto p-4">
    <div class="flex items-center justify-center">
      <div class="fixed-display">
        <!-- Base face layer -->
        <img 
          v-if="selectedFace"
          :src="selectedFace.image" 
          alt="face"
          class="w-48 h-48 absolute"
        />
        <!-- Eyes layer -->
        <img 
          v-if="selectedEyes"
          :src="selectedEyes.image" 
          alt="eyes"
          class="w-32 h-32 absolute z-10 eyes-position"
        />
        <!-- Mouth layer -->
        <img 
          v-if="selectedMouth"
          :src="selectedMouth.image" 
          alt="mouth"
          class="w-32 h-32 absolute z-10 mouth-position"
        />
      </div>
    </div>

    <CharacterMenu
      :activeMenu="activeMenu"
      @update:menu="activeMenu = $event"
      @select="handleSelection"
    />
  </div>
</template>

<style scoped>
.fixed-display {
  min-height: 300px;
  width: 300px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border: 1px solid #e5e7eb;
  border-radius: 0.5rem;
  background-color: white;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.eyes-position {
  transform: translateY(-25px); /* Adjust this value to move eyes up/down */
}

.mouth-position {
  transform: translateY(30px); /* Adjust this value to move mouth up/down */
}
</style>