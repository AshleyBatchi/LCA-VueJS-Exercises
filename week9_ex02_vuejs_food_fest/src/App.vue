<script setup>
import { ref, computed } from "vue";

const favourites = ref(0);

const showFeaturedOnly = ref(false);

const tickets = ref([
  {
    id: 1,
    name: "Bronze",
    price: 150,
    description: "Perfect for casual food lovers.",
    benefits: [
      "General Entry",
      "Street Food Access",
      "Live Music"
    ],
    featured: false,
    favourite: false
  },
  {
    id: 2,
    name: "Silver",
    price: 300,
    description: "More food and exclusive experiences.",
    benefits: [
      "Priority Entry",
      "Chef Demos",
      "VIP Seating"
    ],
    featured: true,
    favourite: false
  },
  {
    id: 3,
    name: "Gold",
    price: 500,
    description: "Ultimate Food Fest experience.",
    benefits: [
      "Unlimited Tastings",
      "Backstage Access",
      "Meet The Chefs",
      "VIP Lounge"
    ],
    featured: false,
    favourite: false
  }
]);

const displayedTickets = computed(() => {
  if (showFeaturedOnly.value) {
    return tickets.value.filter(ticket => ticket.featured);
  }
  return tickets.value;
});

function toggleFavourite(ticket) {
  ticket.favourite = !ticket.favourite;

  if (ticket.favourite) {
    favourites.value++;
  } else {
    favourites.value--;
  }
}
</script>

<template>
  <div class="container">

    <header>
      <h1>🍔 Cape Town Food Fest 2025</h1>
      <p>
        Explore our ticket packages and choose your perfect festival experience.
      </p>

      <h2>❤️ Favourites: {{ favourites }}</h2>

      <button @click="showFeaturedOnly = !showFeaturedOnly">
        {{
          showFeaturedOnly
            ? "Show All Tickets"
            : "Show Featured Only"
        }}
      </button>
    </header>

    <div class="cards">

      <div
        v-for="ticket in displayedTickets"
        :key="ticket.id"
        class="card"
        :class="{ featured: ticket.featured }"
      >

        <h2>{{ ticket.name }}</h2>

        <h3>R{{ ticket.price }}</h3>

        <p>{{ ticket.description }}</p>

        <ul>
          <li
            v-for="benefit in ticket.benefits"
            :key="benefit"
          >
            ✔ {{ benefit }}
          </li>
        </ul>

        <p v-if="ticket.featured">
          ⭐ Featured Tier
        </p>

        <button @click="toggleFavourite(ticket)">
          {{ ticket.favourite ? "❤️ Saved" : "🤍 Favourite" }}
        </button>

        <button>
          Buy Now
        </button>

      </div>

    </div>
  </div>
</template>

<style scoped>
body{
  margin:0;
}

.container{
  max-width:1200px;
  margin:auto;
  padding:20px;
  font-family:Arial,sans-serif;
}

header{
  text-align:center;
  margin-bottom:30px;
}

.cards{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(280px,1fr));
  gap:20px;
}

.card{
  border:1px solid #ddd;
  border-radius:12px;
  padding:20px;
  transition:.3s;
}

.card:hover{
  transform:translateY(-6px);
}

.featured{
  border:3px solid orange;
  box-shadow:0 0 15px orange;
}

button{
  margin-top:10px;
  margin-right:10px;
  padding:10px 15px;
  cursor:pointer;
  border:none;
  border-radius:8px;
}
</style>