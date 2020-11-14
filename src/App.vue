<template>
  <v-app>
    <div v-resize="onResize">
      <v-header
        @nova-consulta="foundUser = {}"
        :foundUser="foundUser"
        class="mb-3"
      />
      <div>
        <v-overlay :value="loading">
          <v-progress-circular indeterminate size="64"></v-progress-circular>
        </v-overlay>
        <div class="d-flex justify-center">
          <v-text-field
            outlined
            label="Buscar usuário"
            prepend-inner-icon="mdi-magnify"
            class="col-10 align-self-center"
            v-model="userToFind"
            @keypress.enter="searchUser"
            v-show="!foundUser.name"
            @click:prepend-inner="searchUser"
          />
        </div>
        <div v-if="foundUser.name">
          <div class="d-flex flex-column flex-md-row">
            <div
              :class="windowWidth < 960 ? 'align-center' : ''"
              class="d-flex flex-row flex-md-column col-12 col-md-3"
            >
              <img
                :src="foundUser.avatar_url"
                class="avatar align-self-center"
              />
              <div class="column">
                <div class="text-center font-weight-black text-h6">
                  {{ foundUser.name }}
                </div>
                <div class="text-center">
                  {{ foundUser.bio }}
                </div>
              </div>
            </div>
            <v-tabs :fixed-tabs="windowWidth < 600">
              <v-tabs-slider color="orange darken-3"></v-tabs-slider>
              <v-tab
                >Repos <v-chip small>{{ foundUser.public_repos }}</v-chip>
              </v-tab>
              <v-tab
                >Starred <v-chip small>{{ starreds.length }}</v-chip></v-tab
              >
              <v-tab-item>
                <repos :repositories="repositories" />
              </v-tab-item>
              <v-tab-item>
                <starred :starreds="starreds" />
              </v-tab-item>
            </v-tabs>
          </div>
        </div>
      </div>
    </div>
  </v-app>
</template>

<script>
import vHeader from "@/components/VAppBar";
import starred from "@/components/Starred";
import repos from "@/components/Repos";
export default {
  name: "App",

  components: {
    vHeader,
    starred,
    repos
  },

  data: () => ({
    userToFind: null,
    repositories: null,
    foundUser: {},
    starreds: [],
    filter: "",
    windowWidth: "",
    loading: false
  }),
  created() {
    this.onResize();
  },
  methods: {
    goToRepository(i) {
      window.location = this.repositories[i].html_url;
    },
    onResize() {
      this.windowWidth = window.innerWidth;
    },
    async searchUser() {
      this.loading = true;
      this.foundUser = await this.axios(`/${this.userToFind}`).then(
        response => response.data
      );
      this.repositories = await this.axios(this.foundUser.repos_url).then(
        response => response.data
      );
      this.starreds = await this.axios(`/${this.foundUser.login}/starred`).then(
        response => response.data
      );
      this.loading = false;
    }
  }
};
</script>
<style>
@media (min-width: 960px) {
  .avatar {
    width: 200px;
  }
}
@media (max-width: 960px) {
  .avatar {
    width: 110px;
  }
}
.avatar {
  border-radius: 50%;
}
.cursor-pointer {
  cursor: pointer;
}
</style>
