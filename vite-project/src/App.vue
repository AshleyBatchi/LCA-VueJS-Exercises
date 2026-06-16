<template>
  <div class="app-wrapper">
    <header class="app-header">
      <h1>FlexZone Fitness Scheduler</h1>
      <p class="counter">Total Sessions Scheduled: <strong>{{ totalSessions }}</strong></p>
    </header>

    <main class="main-layout">
      <section class="form-section">
        <ClassForm @add-session="saveSession" />
      </section>

      <section class="list-section">
        <h3>Current Schedule</h3>
        
        <input 
          type="text" 
          v-model="searchQuery" 
          placeholder="🔍 Search by class or coach name..." 
          class="search-bar"
        />

        <div v-if="filteredSessions.length === 0" class="empty-state">
          <p>🏋️ No fitness sessions scheduled. Use the form to set up your first class!</p>
        </div>

        <div v-else>
          <ClassCard 
            v-for="session in filteredSessions" 
            :key="session.id" 
            :item="session" 
            @delete-session="removeSession"
          />
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import ClassForm from './components/ClassForm.vue';
import ClassCard from './components/ClassCard.vue';

export default {
  name: 'App',
  components: {
    ClassForm,
    ClassCard
  },
  data() {
    return {
      sessions: [],
      searchQuery: ''
    };
  },
  computed: {
    totalSessions() {
      return this.sessions.length;
    },
    filteredSessions() {
      return this.sessions.filter(session => {
        const query = this.searchQuery.toLowerCase();
        return (
          session.name.toLowerCase().includes(query) ||
          session.coach.toLowerCase().includes(query)
        );
      });
    }
  },
  methods: {
    saveSession(newSession) {
      this.sessions.push(newSession);
      localStorage.setItem('flexzone_schedule', JSON.stringify(this.sessions));
    },
    removeSession(sessionId) {
      this.sessions = this.sessions.filter(item => item.id !== sessionId);
      localStorage.setItem('flexzone_schedule', JSON.stringify(this.sessions));
    }
  },
  mounted() {
    const savedData = localStorage.getItem('flexzone_schedule');
    if (savedData) {
      this.sessions = JSON.parse(savedData);
    }
  }
};
</script>

<style>
body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f1faee;
  color: #333;
}
.app-wrapper {
  max-width: 1100px;
  margin: 0 auto;
  padding: 20px;
}
.app-header {
  text-align: center;
  margin-bottom: 30px;
  background: #1d3557;
  color: white;
  padding: 20px;
  border-radius: 8px;
}
.main-layout {
  display: flex;
  gap: 30px;
  flex-wrap: wrap;
}
.form-section {
  flex: 1;
  min-width: 320px;
}
.list-section {
  flex: 2;
  min-width: 350px;
}
.search-bar {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  font-size: 15px;
}
.empty-state {
  background: #e63946;
  color: white;
  padding: 20px;
  text-align: center;
  border-radius: 6px;
}
</style>