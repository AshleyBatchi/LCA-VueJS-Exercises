<script setup>
import { ref } from 'vue'

defineProps({
  recipe: {
    type: Object,
    required: true
  }
})

const isOpen = ref(false)
</script>

<template>
  <div class="recipe-display-card">
    <div class="image-container-wrapper">
      <img :src="recipe.image" :alt="recipe.title" class="card-display-img" />
      <span class="category-card-badge">{{ recipe.category }}</span>
    </div>
    
    <div class="card-body-wrapper">
      <h4>{{ recipe.title }}</h4>
      <p class="summary-text">{{ recipe.description }}</p>
      
      <button @click="isOpen = !isOpen" class="toggle-details-btn">
        {{ isOpen ? 'Hide Blueprint' : 'Inspect Ingredients & Steps' }}
      </button>

      <div v-if="isOpen" class="expanded-data-tray">
        <div class="tray-section">
          <h5>🛒 Required Items:</h5>
          <ul>
            <li v-for="(item, index) in recipe.ingredients" :key="index">
              {{ item }}
            </li>
          </ul>
        </div>
        
        <div class="tray-section" v-if="recipe.instructions">
          <h5>🔥 Preparation Guide:</h5>
          <p class="recipe-method-paragraph">{{ recipe.instructions }}</p>
        </div>
      </div>
    </div>
  </div>
</template>