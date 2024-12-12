<template>
  <div class="dictionary-container">
  <h1>Từ Điển</h1>
    <div class="search-box">
      <input
        v-model="word"
        @input="fetchTranslation"
        type="text"
        placeholder="Enter word"
      />
      <button @click="fetchTranslation">Tìm kiếm</button>
    </div>
    <div class="result">
      <p v-if="translation">{{ translation }}</p>
      <p v-else-if="errorMessage">{{ errorMessage }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      word: "",
      translation: "",
      errorMessage: "",
    };
  },
  methods: {
    async fetchTranslation() {
      if (this.word.trim() === "") {
        this.translation = "";
        this.errorMessage = "";
        return;
      }

      try {
        const response = await axios.get(
          `http://localhost:8080/dictionary?word=${this.word}`
        );
        if (response.status === 200) {
          this.translation = response.data;
          this.errorMessage = "";
        }
      } catch (error) {
        this.translation = "";
        this.errorMessage = "Không tìm thấy từ.";
      }
    },
  },
};
</script>

<style scoped>
.dictionary-container {
  text-align: center;
  padding: 20px;
}

.search-box {
  margin-bottom: 20px;
}

input {
  padding: 8px;
  font-size: 16px;
  margin-right: 10px;
}

button {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
}

.result {
  margin-top: 20px;
  font-size: 18px;
  color:rgb(0, 135, 43);
}

p {
  font-size: 18px;
  font-weight: bold;
}
</style>
