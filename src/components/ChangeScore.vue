<script setup>
import { ref, onMounted } from 'vue';

const selectedTeam = ref(null);
const score = ref(null);
let socket = null; // Define the socket variable

const teams = [
  { name: 'Clix' },
  { name: 'Bugha' },
  { name: 'Ninja' },
  // Add more teams as needed
];

onMounted(() => {
  // Connect to WebSocket server
  socket = new WebSocket('ws://localhost:3000/primus'); // Use 'ws' instead of 'http'
});

const updateStatistics = () => {
  const updatedStats = {
    "action": 'updateStats',
    "team": selectedTeam.value,
    "score": score.value,
  };

  // Send the updated statistics to the WebSocket server
  socket.send(JSON.stringify( updatedStats ));

  // Reset the form fields
  selectedTeam.value = null;
  score.value = null;
};
</script>

<template>
  <div>
    <h2>Update Statistics</h2>
      <label for="team">Select Team:</label>
      <select v-model="selectedTeam" id="team">
        <option v-for="team in teams">{{ team.name }}</option>
      </select>

      <label for="score">Enter Score:</label>
      <input v-model="score" type="number" id="score" required>

      <button type="submit" @click="updateStatistics" >Update Statistics</button>
  </div>
</template>

<style scoped>
/* Your styles here */
</style>
