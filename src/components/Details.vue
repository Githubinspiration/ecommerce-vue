<template>
  <div class="center examplex " style="position:relative; ">
    <vs-navbar v-model="active">
      <vs-navbar-item :active="active == 'followers'" id="followers">
        followers ({{ follow || 0 }})
      </vs-navbar-item>
      <vs-navbar-item :active="active == 'following'" id="following">
        following ({{ followed || 0 }})
      </vs-navbar-item>
    </vs-navbar>
    <div class="bottomDetails">
      <vs-card-group v-if="following.length > 0">
        <vs-card
          :key="index"
          v-for="(data, index) in following"
          @click="viewthisProfile(data.login)"
          type="4"
        >
          <template #title>
            <h3>{{ data.name || data.login }}</h3>
          </template>
          <template #img>
            <img :src="data.avatar_url" alt="" />
          </template>
          <template #text>
            <a @click.prevent="openUrlInNewTab(data.html_url)">
              view github <br />
            </a>
          </template>
        </vs-card>
      </vs-card-group>
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
  data() {
    return {
      active: "followers",
      following: [],
      follow: 0,
      followed: 0,
    };
  },
  async mounted() {
    await this.getFollowers();
    this.updateFollow();
  },
  methods: {
    updateFollow() {
      this.follow = this?.data?.data?.followers || 0;
      this.followed = this?.data?.data?.following || 0;
    },
    async getFollowing() {
      this.following = [];
      let res = await axios(
        `https://api.github.com/users/${this.data?.data?.login}/following`
      );
      this.following = res.data;
    },
    async getFollowers() {
      this.following = [];
      let res = await axios(
        `https://api.github.com/users/${this.data?.data?.login}/followers`
      );
      this.following = res.data;
    },
    openUrlInNewTab: function(link) {
      window.open(link, "_blank");
    },
    viewthisProfile(id) {
      this.following = [];
      this.$emit("new-search", id);
    },
  },
  watch: {
    async active(newActiveValue) {
      if (newActiveValue === "dataChanged") {
        this.active = "followers";
      } else if (newActiveValue === "following") {
        await this.getFollowing();
      } else {
        await this.getFollowers();
      }
    },
    data: {
      async handler() {
        // await this.getFollowers();
        this.active = "dataChanged";
        this.updateFollow();
      },
      deep: true,
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
