<script setup>
import { ref } from "vue";
import Chart from "./Chart.vue";

// reactive state
const symbol = ref("NIFTY");
const interval = ref("1D");
const data = ref([]);

// functions that mutate state and trigger updates
const submitFormHandler = async (e) => {
  e.preventDefault();

  const ticker = symbol.value;
  const period = interval.value;

  const res = await fetch(
    `http://localhost:8000/api/search?symbol=${ticker}&period=${period}`
  );

  data.value = await res.json();
};
</script>

<template>
  <form @submit="submitFormHandler">
    <div class="inputs">
      <div class="input">
        <label for="symbol">Symbol: </label>
        <input id="symbol" type="text" v-model="symbol" required />
      </div>
      <div class="input">
        <label for="interval">Interval: </label>
        <select v-model="interval" id="interval" name="interval">
          <option value="1D">Daily</option>
          <option value="1H">1 Hour</option>
          <option value="2H">2 Hours</option>
          <option value="4H">4 Hours</option>
        </select>
      </div>
    </div>
    <div>
      <button type="submit">Fetch Data</button>
    </div>
  </form>
  <div class="plot" v-if="data.length > 0">
    <Chart :data="data" :autosize="true" />
  </div>
</template>

<style scoped>
form {
  border: 1px solid rgba(120, 60, 152, 0.8);
  border-radius: 10px;
  background-color: rgba(0, 0, 0, 0.2);
  height: 40%;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}

.input {
  display: flex;
  font-size: 28px;
  margin: 10px;
  align-items: center;
}

input,
select {
  width: 60%;
  font-size: 24px;
  padding: 0;
  margin: 4px;
}

button {
  padding: 4px;
  color: black;
  background-color: rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  border-color: rgba(0, 0, 0, 0);
  box-shadow: 2px 2px rgba(0, 0, 0, 0.6);
}

.plot {
  margin-top: 25px;
  height: 400px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

@media all and (max-width: 600px) {
  form {
    border-radius: 0px;
  }
}

@media all and (min-width: 600px) {
  form {
    width: 75%;
  }
}
@media all and (min-width: 800px) {
  form {
    width: 60%;
  }
}

@media all and (min-width: 1200px) {
  form {
    width: 40%;
  }
}
</style>
