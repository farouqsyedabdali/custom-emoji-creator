<script setup>
import { ref } from "vue";
import CharacterMenu from "./CharacterMenu.vue";
import defaultFace from "../assets/defaultface.svg";

const selectedFace = ref(null);
const selectedEyes = ref(null);
const selectedMouth = ref(null);

const activeMenu = ref("faces");

const handleSelection = (option) => {
  if (option.type === "face") {
    selectedFace.value = option;
  } else if (option.type === "eyes") {
    selectedEyes.value = option;
  } else if (option.type === "mouth") {
    selectedMouth.value = option;
  }
};

const downloadPNG = async () => {
  const canvas = document.createElement("canvas");
  const ctx = canvas.getContext("2d");

  canvas.width = 400;
  canvas.height = 300;

  ctx.fillStyle = "white";
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  const loadImage = (src) => {
    return new Promise((resolve, reject) => {
      const img = new Image();
      img.onload = () => resolve(img);
      img.onerror = reject;
      img.src = src;
    });
  };

  try {
    if (selectedFace.value) {
      const face = await loadImage(selectedFace.value.image);
      ctx.drawImage(face, 50, 50, 300, 300);
    }

    if (selectedEyes.value) {
      const eyes = await loadImage(selectedEyes.value.image);
      ctx.drawImage(eyes, 50, 50, 300, 300);
    }

    if (selectedMouth.value) {
      const mouth = await loadImage(selectedMouth.value.image);
      ctx.drawImage(mouth, 50, 50, 300, 300);
    }

    // Create download link
    const link = document.createElement("a");
    link.download = "character.png";
    link.href = canvas.toDataURL("image/png");
    link.click();

  } catch (error) {
    console.error("Error generating PNG:", error);
  }
};

</script>

<template>
  <div class="grid grid-cols-2 gap-4 container mx-auto p-4">
    <div class="flex items-center justify-center">
      <div class="fixed-display">
        <img
          v-if="selectedFace"
          :src="selectedFace.image"
          alt="face"
          class="w-48 h-48 absolute"
        />
        <img
          v-if="selectedEyes"
          :src="selectedEyes.image"
          alt="eyes"
          class="w-32 h-32 absolute z-10 eyes-position"
        />
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
    <button 
        @click="downloadPNG"
        class="btn btn-primary mt-4 w-max m-auto"
        :disabled="!selectedFace"
      >
        Download PNG
      </button>
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
  transform: translateY(-25px);
}

.mouth-position {
  transform: translateY(30px);
}
</style>
