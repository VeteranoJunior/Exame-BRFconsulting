<template>
  <div id="app">
    <div class="container">
      <div class="input-group">
        <input
          type="text"
          class="form-control"
          aria-label="Dollar amount (with dot and two decimal places)"
          v-model="country"
        />
        <button type="button" @click="search" class="btn btn-primary btn-lg">
          Search
        </button>
      </div>
      <h2 class="text-muted" v-if="erro" style="color: red">
        No country found, please write the full name
      </h2>
      <div v-if="start">
        <div v-for="(item, idx) in start.slice(0, 10)" :key="idx" class="card m-2" style="width: 400px">
          <div class="card-body row">
            <img class="col" :src="item.flag" style="width: 50%; border-radius: 50%" />
            <h2 class="col text-center mt-5">{{ item.name }}</h2>
          </div>
        </div>
      </div>

      <!-- CARD -->
      <div v-if="info" class="card m-2" style="width: 400px">
        <div class="card-body row">
          <img class="col" :src="flag" style="width: 50%; border-radius: 50%" />
          <h2 class="col text-center mt-5">{{ info }}</h2>
        </div>
      </div>
      <!-- <pre>{{ start.slice(0, 20) }}</pre> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  components: {},
  data() {
    return {
      info: null,
      country: "",
      flag: "",
      erro: false,
      start: null,
    };
  },
  created() {
      axios
        .get("https://restcountries.eu/rest/v2/all")
        .then((response) => {
          this.start = response.data
        })
  },
  methods: {
    search() {
      this.start = null
      axios
        .get(`https://restcountries.eu/rest/v2/name/${this.country}?fullText=true`)
        .then((response) => {
            (this.info = response.data[0].name),
            (this.flag = response.data[0].flag);
        })
        .catch((error) => {
          console.log(error);
          this.info = null;
          this.erro = true;
          setTimeout(() => {
            this.erro = false;
          }, 2000);
        });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  margin-top: 60px;
}
</style>
