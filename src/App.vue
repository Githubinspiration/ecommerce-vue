<template>
  <div class="main" style=" height:100vh;">
    <Header />

    <div class="center content-inputs ">
      <div style="display:flex;  justify-content: center;">
        <div>madeel20</div>
        <div>owais897</div>
      </div>

      <br />
      <div class="inputField">
        <vs-input
          type="text"
          v-model="getId"
          name="text"
          placeholder="Enter Github Id"
          color="#7d33ff"
        />
        <vs-button @click.prevent="onSubmit">submit</vs-button>
      </div>
    </div>

    <ShowDetails
      @new-search2="showThisUser($event, data)"
      :data="data"
      :loading="loading"
    />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import ShowDetails from "./components/showDetails.vue";
import axios from "axios";
export default {
  name: "App",
  components: {
    Header,
    ShowDetails,
  },
  data() {
    return {
      getId: "",
      data: {},
      loading: false,
    };
  },
  methods: {
    async onSubmit() {
      if (!this.getId) {
        alert("Please add a id");
        return;
      }
      this.loading = true;
      let data = await this.getUserData(this.getId);
      setTimeout(() => {
        //your code to be executed after 1 second
        this.loading = false;
      }, 1000);
      this.data = data;
    },
    showThisUser(id) {
      this.getId = id;
      this.onSubmit();
    },
    async getUserData(id) {
      let data = await axios.get(`https://api.github.com/users/${id}`);
      return data;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.inputField {
  display: flex;
  justify-content: center;
  font-size: 20px;
  margin-bottom: 50px;
}
.main {
  background-color: #f5f2e2;
  /* background-image: url("https://github.githubassets.com/images/modules/site/home/globe.jpg"); */
}
</style>
