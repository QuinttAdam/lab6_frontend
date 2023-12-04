<script setup>

import { ref, onMounted } from 'vue';

let socket= null;
let data= ref([{
    team: "Tfue",
    score: "5",
}, {
    team: "AsianJeff",
    score: "3",
}]);

onMounted(() => {
  // Connect to WebSocket server
  socket = new WebSocket('wss://labo6-backend.onrender.com/primus'); // Use 'ws' instead of 'http'

  socket.onmessage= (event)=>{
        // console.log(event.data);
        let newScore= JSON.parse(event.data);
        console.log(newScore);
        if(newScore.data.action==="updateStats"){
            data.value.push({
                team: newScore.data.team,
                score: newScore.data.score,
            });
        }
    }
});
</script>

<template>
  <div>
    <ul>
        <li v-for="item in data">
            {{ item.team }}: {{ item.score }}
        </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
