<template>
  <form @submit.prevent="handleSubmit" class="form-container">
    <h3>Add New Class Session</h3>
    
    <div class="form-group">
      <label>Class Name:</label>
      <input type="text" v-model="form.name" placeholder="e.g., Power Yoga" />
    </div>

    <div class="form-group">
      <label>Instructor / Coach:</label>
      <input type="text" v-model="form.coach" placeholder="e.g., Coach Sarah" />
    </div>

    <div class="form-group row">
      <div>
        <label>Date:</label>
        <input type="date" v-model="form.date" />
      </div>
      <div>
        <label>Time:</label>
        <input type="time" v-model="form.time" />
      </div>
    </div>

    <div class="form-group">
      <label>Max Capacity:</label>
      <input type="number" v-model.number="form.capacity" min="1" placeholder="e.g., 20" />
    </div>

    <p v-if="errorMessage" class="error-msg">{{ errorMessage }}</p>

    <button type="submit" class="btn-submit">Schedule Session</button>
  </form>
</template>

<script>
export default {
  name: 'ClassForm',
  emits: ['add-session'],
  data() {
    return {
      form: {
        name: '',
        coach: '',
        date: '',
        time: '',
        capacity: ''
      },
      errorMessage: ''
    };
  },
  methods: {
    handleSubmit() {
      // Direct validation using standard reactivity reactive logic
      if (!this.form.name || !this.form.coach || !this.form.date || !this.form.time || !this.form.capacity) {
        this.errorMessage = '⚠️ All fields are required. Please complete the form.';
        return;
      }
      
      if (this.form.capacity <= 0) {
        this.errorMessage = '⚠️ Capacity must be at least 1 person.';
        return;
      }

      this.errorMessage = '';
      
      // Emit data up to App.vue with a unique timestamp ID
      this.emit('add-session', {
        id: Date.now(),
        ...this.form
      });

      // Reset the input fields completely
      this.form.name = '';
      this.form.coach = '';
      this.form.date = '';
      this.form.time = '';
      this.form.capacity = '';
    }
  }
};
</script>

<style scoped>
.form-container {
  background: #ffffff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  margin-bottom: 25px;
}
.form-group {
  margin-bottom: 15px;
  display: flex;
  flex-direction: column;
}
.row {
  flex-direction: row;
  gap: 15px;
}
.row div {
  flex: 1;
  display: flex;
  flex-direction: column;
}
label {
  font-weight: 600;
  margin-bottom: 5px;
  color: #333;
}
input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
}
.error-msg {
  color: #e63946;
  font-weight: bold;
  margin-bottom: 10px;
}
.btn-submit {
  width: 100%;
  background: #1d3557;
  color: white;
  padding: 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
  font-size: 16px;
}
.btn-submit:hover {
  background: #457b9d;
}
</style> 