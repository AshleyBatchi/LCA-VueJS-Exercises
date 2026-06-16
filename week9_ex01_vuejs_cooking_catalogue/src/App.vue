<script setup>
import { ref, computed } from 'vue'
import RecipeCard from './components/RecipeCard.vue'

// Base array containing default recipe entries
const recipes = ref([
  {
    id: 1,
    title: "Traditional South African Bobotie",
    category: "Dinner",
    image: "https://images.unsplash.com/photo-1606787366850-de6330128bfc?w=500&auto=format&fit=crop&q=60",
    description: "A rich, classic South African baked dish featuring spiced minced meat topped with a creamy egg custody layer.",
    ingredients: ["500g Minced Beef", "1 Cup Milk", "2 Slices White Bread", "1 Chopped Onion", "2 tbsp Curry Powder", "2 Eggs"],
    instructions: "Soak the bread in milk. Sauté onions and curry powder, then brown the minced beef. Mix in the soaked bread. Transfer the mixture to a greased baking pan, pour beaten egg mixture over the top, and bake at 180°C for roughly 30 minutes."
  },
  {
    id: 2,
    title: "Buttermilk Pancake Stack",
    category: "Breakfast",
    image: "https://images.unsplash.com/photo-1528207776546-365bb710ee93?w=500&auto=format&fit=crop&q=60",
    description: "Perfectly light, fluffy golden breakfast hotcakes best served fresh with warm maple syrup or fruit toppings.",
    ingredients: ["2 Cups Cake Flour", "2 tbsp Castor Sugar", "2 tsp Baking Powder", "1 Large Egg", "1.5 Cups Buttermilk", "50g Melted Butter"],
    instructions: "Whisk all dry ingredients in a bowl. Blend wet elements together separately before combining gently. Spoon batter onto a heated non-stick frying pan. Flip once surface bubbles begin to burst and cook until golden brown."
  },
  {
    id: 3,
    title: "Crisp Parmesan Caesar Salad",
    category: "Lunch",
    image: "https://images.unsplash.com/photo-1550304943-4f24f54ddde9?w=500&auto=format&fit=crop&q=60",
    description: "Fresh, crunchy Romaine lettuce tossed with a rich homemade Caesar dressing, crunchy croutons, and fine cheese shavings.",
    ingredients: ["1 Large Romaine Lettuce Head", "1/2 Cup Crunchy Croutons", "1/4 Cup Grated Parmesan Cheese", "4 tbsp Creamy Caesar Dressing"],
    instructions: "Thoroughly rinse and chop the Romaine lettuce. Toss leaves with the dressing in a clean serving bowl. Garnish evenly with the baked bread croutons and fresh parmesan cheese shavings immediately before serving cold."
  }
])

// Filter tracking reactivity variables
const searchQuery = ref('')
const selectedCategory = ref('All')
const categories = ['All', 'Breakfast', 'Lunch', 'Dinner']

// Interactive Add Recipe form model variables
const newTitle = ref('')
const newCategory = ref('Dinner')
const newDescription = ref('')
const newIngredients = ref('')
const newInstructions = ref('')

// Computed filtering handler logic
const filteredRecipes = computed(() => {
  return recipes.value.filter(recipe => {
    const matchSearch = recipe.title.toLowerCase().includes(searchQuery.value.toLowerCase()) || 
                        recipe.description.toLowerCase().includes(searchQuery.value.toLowerCase())
    const matchCategory = selectedCategory.value === 'All' || recipe.category === selectedCategory.value
    return matchSearch && matchCategory
  })
})

// Submits a new dish directly to the reactive list array
const handleAddRecipe = () => {
  if (!newTitle.value || !newDescription.value) return

  recipes.value.push({
    id: Date.now(),
    title: newTitle.value,
    category: newCategory.value,
    image: "https://images.unsplash.com/photo-1495521821757-a1efb6729352?w=500&auto=format&fit=crop&q=60", 
    description: newDescription.value,
    ingredients: newIngredients.value.split(',').map(i => i.trim()).filter(Boolean),
    instructions: newInstructions.value
  })

  // Complete field reset clearing process
  newTitle.value = ''
  newDescription.value = ''
  newIngredients.value = ''
  newInstructions.value = ''
}
</script>

<template>
  <div class="app-container">
    <header class="app-header">
      <h1>🍳 LCA Culinary Catalogue</h1>
      <p>Discover delicious recipes, filter through meal options, or add your own creations.</p>
    </header>

    <main class="dashboard-grid">
      <!-- Controls Layout Section Column -->
      <section class="control-panel">
        <div class="filter-card">
          <h3>Search & Filter</h3>
          <input 
            v-model="searchQuery" 
            type="text" 
            placeholder="Type keyword to find..." 
            class="input-field"
          />
          <div class="category-pill-group">
            <button 
              v-for="cat in categories" 
              :key="cat"
              @click="selectedCategory = cat"
              :class="['pill-btn', { active: selectedCategory === cat }]"
            >
              {{ cat }}
            </button>
          </div>
        </div>

        <div class="filter-card">
          <h3>Contribute a Recipe</h3>
          <form @submit.prevent="handleAddRecipe" class="recipe-form">
            <div class="form-item">
              <label>Recipe Title</label>
              <input v-model="newTitle" type="text" required placeholder="e.g., Spicy Chicken Tacos" class="input-field" />
            </div>
            <div class="form-item">
              <label>Meal Category</label>
              <select v-model="newCategory" class="input-field">
                <option value="Breakfast">Breakfast</option>
                <option value="Lunch">Lunch</option>
                <option value="Dinner">Dinner</option>
              </select>
            </div>
            <div class="form-item">
              <label>Brief Description</label>
              <textarea v-model="newDescription" required placeholder="Write a short teaser text..." class="input-field text-area"></textarea>
            </div>
            <div class="form-item">
              <label>Ingredients (Separate entries with commas)</label>
              <input v-model="newIngredients" type="text" placeholder="Flour, Sugar, Warm Water..." class="input-field" />
            </div>
            <div class="form-item">
              <label>Cooking Steps</label>
              <textarea v-model="newInstructions" placeholder="Describe the method step by step..." class="input-field text-area"></textarea>
            </div>
            <button type="submit" class="submit-action-btn">Add to Catalogue</button>
          </form>
        </div>
      </section>

      <!-- Active Grid Cards List View -->
      <section class="display-panel">
        <div v-if="filteredRecipes.length === 0" class="empty-layout-alert">
          <p>No culinary matches found matching your keyword filters.</p>
        </div>
        <div v-else class="catalogue-responsive-grid">
          <RecipeCard 
            v-for="recipe in filteredRecipes" 
            :key="recipe.id" 
            :recipe="recipe" 
          />
        </div>
      </section>
    </main>
  </div>
</template>