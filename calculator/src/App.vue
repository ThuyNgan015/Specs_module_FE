<template>
  <div id="app" class="calculator-container">
    <h1>Calculator</h1>
    <div class="calculator-form">
      <div class="input-group">
        <label for="firstNumber">First Number:</label>
        <input type="text" id="firstNumber" v-model="firstNumber" />
      </div>
      <div class="input-group">
        <label for="secondNumber">Second Number:</label>
        <input type="text" id="secondNumber" v-model="secondNumber" />
      </div>
      <div class="input-group">
        <label for="operator">Operator:</label>
        <select id="operator" v-model="operator">
          <option value="+">+</option>
          <option value="-">-</option>
          <option value="*">*</option>
          <option value="/">/</option>
        </select>
      </div>
      <button class="calculate-button" @click="calculate">Submit</button>
    </div>

    <div v-if="result" class="result-display">
      <h2>Result:</h2>
      <p>{{ result }}</p>
    </div>

    <div v-if="error" class="error-display">
      <h2>Error:</h2>
      <p>{{ error }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Calculator-app',
  data() {
    return {
      firstNumber: '',
      secondNumber: '',
      operator: '+',
      result: '',
      error: ''
    };
  },
  methods: {
    async calculate() {
      this.result = '';
      this.error = '';
      try {
        const response = await axios.get('http://localhost:8080/calculator', {
          params: {
            firstNumber: this.firstNumber,
            secondNumber: this.secondNumber,
            operator: this.operator
          }
        });
        this.result = response.data;
      } catch (err) {
        this.error = err.response?.data || 'An error occurred.';
      }
    }
  }
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f9;
  color: #333;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.calculator-container {
  background: #fff;
  padding: 20px 30px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
  text-align: center;
}

h1 {
  margin-bottom: 20px;
  font-size: 24px;
  color: #4a90e2;
}

.calculator-form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.input-group {
  display: flex;
  flex-direction: column;
  text-align: left;
}

label {
  margin-bottom: 5px;
  font-weight: bold;
}

input, select {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
  outline: none;
}

input:focus, select:focus {
  border-color: #4a90e2;
}

.calculate-button {
  background-color: #4a90e2;
  color: #fff;
  padding: 10px 15px;
  font-size: 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.calculate-button:hover {
  background-color: #357ab8;
}

.result-display, .error-display {
  margin-top: 20px;
  padding: 10px;
  border-radius: 4px;
  text-align: left;
}

.result-display {
  background-color: #e6f7e9;
  color: #2d7a39;
}

.error-display {
  background-color: #fdecea;
  color: #d93025;
}
</style>
