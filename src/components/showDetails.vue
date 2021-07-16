<template>
  <div>
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
          <span> followers: {{ data.data.followers }}<br /> </span>
          <span> following: {{ data.data.following }}<br /> </span>
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
    <Details :data="data" @new-search="method1($event, data)" />
  </div>
</template>

<script>
import moment from "moment";
import Details from "./Details.vue";
export default {
  name: "showCard",
  props: {
    data: Object,
    loading: Boolean,
  },
  components: {
    Details,
  },
  methods: {
    formatDate(d) {
      return moment(new Date(d)).format("MMMM Do YYYY, h:mm:ss a");
    },
    method1(id) {
      this.$emit("new-search2", id);
    },
  },
  data() {
    return { lastCommit: "", joined: "" };
  },
  emits: ["new-search2"],
  computed: {},
  watch: {
    data: {
      handler() {
        this.joined = this.formatDate(this.data.data.created_at);
        this.lastCommit = this.formatDate(this.data.data.updated_at);
      },
      deep: true,
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
