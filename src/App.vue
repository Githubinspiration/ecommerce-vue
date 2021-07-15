<template>
  <div>
    <Header />
    <div>madeel20</div>
    <div>owais897</div>

    <input
      type="text"
      v-model="getId"
      name="text"
      placeholder="Enter Github Id"
    />
    <button @click.prevent="onSubmit">submit</button>

    <ShowDetails :data="data" :loading="loading" />
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
      this.loading = true;
      console.log("trigger submit");

      if (!this.getId) {
        alert("Please add a id");
        return;
      }
      let data = await this.getUserData(this.getId);

      console.log("data: ", data);
      setTimeout(() => {
        //your code to be executed after 1 second
        this.loading = false;
      }, 1000);
      this.data = data;
    },
    async getUserData(id) {
      let data = await axios.get(`https://api.github.com/users/${id}`);
      return data;
    },
  },
};
</script>

<style></style>
