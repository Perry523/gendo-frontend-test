<template>
  <v-card-text>
    <v-text-field
      outlined
      label="Filtrar por nome"
      prepend-inner-icon="mdi-magnify"
      v-model="filter"
      @keypress.enter="filterByName"
      @click:prepend-inner="filterByName"
    />
    <div v-for="(starred, i) in starredRepos" :key="i">
      <a target="_blank" class="text-decoration-none" :href="starred.clone_url">
        <div class="blue--text d-flex text-body-1 cursor-pointer">
          <h3 class="font-weight-regular">{{ starred.owner.login }} /</h3>
          <h2 class="ml-1">{{ starred.name }}</h2>
        </div>
      </a>
      <p class="mt-2">{{ starred.description }}</p>
      <div class="d-flex mt-5">
        <v-icon>mdi-star</v-icon>
        <div class="ml-1 mr-5">
          {{ starred.stargazers_count }}
        </div>
        <img src="@/assets/shape.svg" />
        <div class="ml-1">
          {{ starred.forks_count }}
        </div>
      </div>
      <v-divider class="my-5"></v-divider>
    </div>
  </v-card-text>
</template>

<script>
export default {
  props: {
    starreds: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      filter: "",
      starredRepos: []
    };
  },
  mounted() {
    this.starredRepos = this.starreds;
  },
  methods: {
    filterByName() {
      this.starredRepos = this.starreds.filter(
        starred =>
          starred.name.includes(this.filter) ||
          starred.owner.login.includes(this.filter)
      );
    }
  },
  watch: {
    starreds() {
      this.starredRepos = this.starreds;
    }
  }
};
</script>
