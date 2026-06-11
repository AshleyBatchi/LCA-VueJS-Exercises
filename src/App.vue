<template>
  <div class="app">

    <!-- HEADER -->
    <header class="header">
      <h1>Cooking Masterclass Catalogue</h1>

      <div class="actions">
        <p class="wishlist">Wishlist: {{ wishlist.length }}</p>

        <button class="btn" @click="toggleFilter">
          {{ showAvailableOnly ? "Show All Courses" : "Show Available Only" }}
        </button>
      </div>
    </header>

    <!-- SEARCH (EXTRA MARKS) -->
    <input
      v-model="search"
      placeholder="Search courses..."
      class="search"
    />

    <!-- COURSES -->
    <div class="grid">

      <div
        v-for="course in filteredCourses"
        :key="course.id"
        class="card"
        :class="{ sold: !course.available }"
      >

        <div class="card-header">
          <h2>{{ course.title }}</h2>

          <span v-if="course.available" class="badge available">
            Available
          </span>

          <span v-else class="badge sold-badge">
            Sold Out
          </span>
        </div>

        <p><strong>Chef:</strong> {{ course.chef }}</p>
        <p><strong>Level:</strong> {{ course.level }}</p>
        <p><strong>Price:</strong> R{{ course.price }}</p>

        <!-- BUTTON -->
        <button
          class="save-btn"
          @click="addToWishlist(course)"
          :disabled="!course.available"
        >
          Save to Wishlist
        </button>

      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      wishlist: [],
      showAvailableOnly: false,
      search: "",

      courses: [
        { id: 1, title: "Italian Pasta Mastery", chef: "Marco", level: "Beginner", price: 299, available: true },
        { id: 2, title: "Sushi Essentials", chef: "Sato", level: "Intermediate", price: 399, available: false },
        { id: 3, title: "French Pastry", chef: "Pierre", level: "Advanced", price: 499, available: true },
        { id: 4, title: "BBQ Masterclass", chef: "John", level: "Beginner", price: 199, available: true },
        { id: 5, title: "Street Food Secrets", chef: "Amina", level: "Beginner", price: 149, available: false }
      ]
    };
  },

  computed: {
    filteredCourses() {
      let result = this.courses;

      // FILTER AVAILABLE ONLY
      if (this.showAvailableOnly) {
        result = result.filter(c => c.available);
      }

      // SEARCH FILTER
      if (this.search) {
        result = result.filter(c =>
          c.title.toLowerCase().includes(this.search.toLowerCase()) ||
          c.chef.toLowerCase().includes(this.search.toLowerCase())
        );
      }

      return result;
    }
  },

  methods: {
    addToWishlist(course) {
      if (!this.wishlist.includes(course)) {
        this.wishlist.push(course);
      }
    },

    toggleFilter() {
      this.showAvailableOnly = !this.showAvailableOnly;
    }
  }
};
</script>

<style>
/* GLOBAL */
.app {
  font-family: Arial;
  padding: 20px;
  background: #f5f5f5;
}

/* HEADER */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #111;
  color: white;
  padding: 15px;
  border-radius: 10px;
}

.actions {
  display: flex;
  align-items: center;
  gap: 15px;
}

.wishlist {
  font-weight: bold;
}

/* SEARCH */
.search {
  width: 100%;
  margin-top: 15px;
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #ccc;
}

/* GRID */
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
  margin-top: 20px;
}

/* CARD */
.card {
  background: white;
  padding: 15px;
  border-radius: 10px;
  transition: 0.3s;
  box-shadow: 0 3px 10px rgba(0,0,0,0.1);
}

.card:hover {
  transform: translateY(-5px);
}

/* SOLD OUT */
.sold {
  opacity: 0.6;
}

/* BADGES */
.badge {
  padding: 5px 10px;
  font-size: 12px;
  border-radius: 5px;
}

.available {
  background: green;
  color: white;
}

.sold-badge {
  background: red;
  color: white;
}

/* BUTTON */
.btn {
  padding: 8px 12px;
  border: none;
  background: orange;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}

.save-btn {
  margin-top: 10px;
  width: 100%;
  padding: 10px;
  border: none;
  background: #2e7d32;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}

.save-btn:disabled {
  background: gray;
}

/* RESPONSIVE */
@media (max-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 480px) {
  .grid {
    grid-template-columns: 1fr;
  }

  .header {
    flex-direction: column;
    gap: 10px;
  }
}
</style>