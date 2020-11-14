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
    <div v-for="(repository, i) in repositoriesToShow" :key="i">
      <a
        target="_blank"
        class="text-decoration-none"
        :href="repository.clone_url"
      >
        <div class="blue--text d-flex text-body-1 cursor-pointer">
          <h2>{{ repository.name }}</h2>
        </div>
      </a>
      <p class="mt-3">{{ repository.description }}</p>
      <div class="d-flex mt-5">
        <div v-html="'< >'"></div>
        <div class="ml-1 mr-5">
          {{ repository.language }}
        </div>
        <img src="@/assets/shape.svg" />
        <div class="ml-1">
          {{ repository.forks_count }}
        </div>
      </div>
      <v-divider class="my-5"></v-divider>
    </div>
  </v-card-text>
</template>

<script>
export default {
  data() {
    return {
      filter: "",
      repositoriesToShow: []
    };
  },
  props: {
    repositories: {
      type: Array,
      default: () => []
    }
  },
  mounted() {
    this.repositoriesToShow = this.repositories;
  },
  methods: {
    filterByName() {
      this.repositoriesToShow = this.repositories.filter(
        repo =>
          repo.name.includes(this.filter) ||
          repo.owner.login.includes(this.filter)
      );
    }
  },
  watch: {
    repositories() {
      this.repositoriesToShow = this.repositories;
    }
  }
};
</script>
