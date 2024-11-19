<script setup>
import { computed } from 'vue';
import angryFace from '../assets/angryface.svg';
import defaultFace from '../assets/defaultface.svg';
import purpleFace from '../assets/purpleface.svg';
import wideEyesLook from '../assets/wideeyeslook.svg';
import heartEyes from '../assets/hearteyes.svg';
import starEyes from '../assets/stareyes.svg';
import smile from '../assets/smile.svg';
import lipSmile from '../assets/lipsmile.svg';
import tongueMouth from '../assets/tonguemouth.svg';

const props = defineProps(['activeMenu']);
const emit = defineEmits(['update:menu', 'select']);

const baseOptions = [
  { 
    id: 1,
    image: purpleFace,
    type: 'face'
  },
  { 
    id: 2,
    image: angryFace,
    type: 'face'
  },
  { 
    id: 3,
    image: defaultFace,
    type: 'face'
  },
];

const eyeOptions = [
  { 
    id: 4,
    image: wideEyesLook,
    type: 'eyes'
  },
  {
    id: 5,
    image: heartEyes,
    type: 'eyes'
  },
  {
    id: 6,
    image: starEyes,
    type: 'eyes'
  }
];

const mouthOptions = [
    {
        id: 7,
        image: smile,
        type: 'mouth'
    },
    {
        id: 8,
        image: lipSmile,
        type: 'mouth'
    },
    {
        id: 9,
        image: tongueMouth,
        type: 'mouth'
    }
];

const menuItems = ['faces', 'eyes', 'mouth'];

const options = computed(() => {
  switch(props.activeMenu) {
    case 'faces': return baseOptions;
    case 'eyes': return eyeOptions;
    case 'mouth': return mouthOptions;
    default: return [];
  }
});
</script>

<template>
  <div class="character-menu">
    <ul class="menu menu-vertical lg:menu-horizontal bg-base-200 rounded-box mb-4">
      <li v-for="item in menuItems" :key="item" class="mx-2">
        <a 
          @click="emit('update:menu', item)"
          :class="{ 'active': activeMenu === item }"
        >
          {{ item.charAt(0).toUpperCase() + item.slice(1) }}
        </a>
      </li>
    </ul>

    <div class="options-container">
      <button
        v-for="option in options"
        :key="option.id"
        @click="emit('select', option)"
        class="btn btn-primary m-2"
        :style="{ backgroundColor: option.color }"
      >
        <img :src="option.image" alt="option" class="w-8 h-8" />
        {{ option.name }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.character-menu {
  padding: 1rem;
  background-color: #f8f9fa;
  border-radius: 0.5rem;
}

.options-container {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.active {
  background-color: #ddd;
  font-weight: bold;
}

.btn {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>