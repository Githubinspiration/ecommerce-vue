<template>
  <div class="center examplex " style="position:relative; ">
    <vs-navbar v-model="active">
      <vs-navbar-item :active="active == 'followers'" id="followers">
        followers
      </vs-navbar-item>
      <vs-navbar-item :active="active == 'following'" id="following">
        following
      </vs-navbar-item>
    </vs-navbar>
    <div class="bottomDetails">
      <ul id="example-2">
        <li :key="index" v-for="(data, index) in following">
          <vs-card @click="viewthisProfile(data.login)" type="3">
            <template #title>
              <h3>{{ data.name || data.login }}</h3>
            </template>
            <template #img>
              <img :src="data.avatar_url" alt="" />
            </template>
            <template #text>
              <a @click.prevent="myFunction(data.html_url)">
                view github <br />
              </a>
            </template>
          </vs-card>
        </li>
      </ul>
    </div>
    {{ following }}
    <div>
      55555555555555555555555555
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Details",
  props: {
    data: Object,
    loading: Boolean,
  },
  data: () => ({
    active: "followers",
    following: [],
    userName: undefined,
  }),
  updated() {
    // console.log("previos: ", this.active);
    // this.getFollowing();
  },
  async mounted() {
    console.log("trigger mounted");
    await this.getFollowing();
  },
  created() {
    console.log("trigger created", this.data);
  },
  methods: {
    async getFollowing() {
      let res = await axios(
        `https://api.github.com/users/${this.data?.data?.login}/following`
      );
      this.following = res.data;
    },
    myFunction: function(link) {
      window.open(link, "_blank");
    },
    viewthisProfile(id) {
      console.log("trigger new search");
      this.$emit("new-search", id);
    },
  },
  watch: {
    active(ne, previos) {
      console.log("previos: ", previos);
      console.log("ne: ", ne);
    },
    data: {
      handler() {
        console.log("in data changeeeeeeeeeeee");
        this.getFollowing();
      },
      deep: true,
      // immediate: true,
    },
  },
};
</script>

<style>
.detailsDiv {
  background-color: chocolate;
}
.bottomDetails {
  padding-top: 50px;
  display: flex;
  align-content: center;
}
</style>
