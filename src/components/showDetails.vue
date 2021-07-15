<template>
  <div>
    github profile details
    <div v-show="loading">
      loading
    </div>
    <div v-if="!loading && data.data">
      <span> Name: {{ data.data.name }} <br /> </span
      ><span> company: {{ data.data.company }} <br /> </span
      ><span> location: {{ data.data.location }} <br /> </span
      ><span> public_repos: {{ data.data.public_repos }} <br /> </span
      ><span> following: {{ data.data.following }}<br /> </span
      ><span v-show="data.data.followers">
        followers: {{ data.data.followers }}<br /> </span
      ><span>
        joined github: {{ new Date(data.data.created_at).toString()
        }}<br /> </span
      ><span>
        last commit: {{ new Date(data.data.updated_at).toString("MMMM yyyy")
        }}<br />
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Details",
  props: {
    data: Object,
    loading: Boolean,
  },
  methods: {},
  computed: {
    show() {
      console.log("this.loading: ", this.loading);
      return this.loading;
    },
  },
};
</script>

<style></style>
