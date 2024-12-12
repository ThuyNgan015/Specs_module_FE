<template>
  <div id="app" class="container">
    <form @submit.prevent="fetchGreeting">
      <div>
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="name" placeholder="Enter your name" />
      </div>
      <div>
        <label for="address">Address:</label>
        <input type="text" id="address" v-model="address" placeholder="Enter your address" />
      </div>
      <button type="submit">Greeting</button>
    </form>

    <p v-if="greetingMessage" class="greeting-result">
      {{ greetingMessage }}
    </p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      name: "",
      address: "",
      greetingMessage: "",
    };
  },
  methods: {
    async fetchGreeting() {
      try {
        const response = await axios.get("http://localhost:8080/greeting", {
          params: {
            name: this.name,
            address: this.address,
          },
        });
        this.greetingMessage = response.data;
      } catch (error) {
        console.error("Error fetching greeting:", error);
        this.greetingMessage = "Error fetching greeting. Please try again.";
      }
    },
  },
};
</script>

<style>
.container {
  max-width: 600px;
  margin: 50px auto;
  text-align: center;
}
form {
  margin-bottom: 20px;
}
form div {
  margin: 10px 0;
}
input {
  padding: 10px;
  width: 100%;
  max-width: 300px;
  margin: 5px auto;
}
button {
  padding: 10px 20px;
  cursor: pointer;
}
.greeting-result {
  font-size: 1.2em;
  margin-top: 20px;
  color: #4caf50;
}
</style>
