<template>
  <div>
    <h1>Github profile details</h1>

    <div v-show="loading">
      loading
    </div>
    <div class="centerSty" v-if="!loading && data.data">
      <vs-card type="3">
        <template #title>
          <h3>{{ data.data.name || data.data.login }}</h3>
        </template>
        <template #img>
          <img :src="data.data.avatar_url" alt="" />
        </template>
        <template #text>
          <p>{{ data.data.company }} . {{ data.data.location }}</p>
          <span> following: {{ data.data.following }}<br /> </span
          ><span v-show="data.data.followers">
            followers: {{ data.data.followers }}<br />
          </span>
          <span> public repos: {{ data.data.public_repos }} <br /> </span>
        </template>
      </vs-card>

      <span style="margin-top:10px">
        <b>Joined github:</b>
        {{ joined }}
        <br />
      </span>
      <span> <b>Last commit:</b> {{ lastCommit }} <br /> </span>
    </div>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: "Details",
  props: {
    data: Object,
    loading: Boolean,
  },
  methods: {
    formatDate(d) {
      return moment(new Date(d)).format("MMMM Do YYYY, h:mm:ss a");
    },
  },
  data() {
    return { lastCommit: "", joined: "" };
  },
  computed: {},
  watch: {
    data: {
      handler() {
        this.joined = this.formatDate(this.data.data.created_at);
        this.lastCommit = this.formatDate(this.data.data.updated_at);
      },
      deep: true,
      immediate: true,
    },
  },
};
</script>

<style>
.centerSty {
  display: flex;
  justify-content: center;
  flex-direction: column;
  /* background-color: brown; */
  display: grid;
  height: 100%;
  padding: 30px 0px;
}
</style>
