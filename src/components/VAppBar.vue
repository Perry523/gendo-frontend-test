<template>
  <v-app-bar color="black">
    <img
      src="https://cdn3.iconfinder.com/data/icons/inficons/512/github.png"
      alt="gihhub icon"
      width="40px"
      height="40px"
    />
    <div class="white--text text-h6 mr-auto">
      <b class="ml-2">Github</b>
      <span class="font-weight-light"> profiles</span>
    </div>
    <v-btn
      v-show="foundUser.login"
      @click="$emit('nova-consulta')"
      color="primary"
      >resetar</v-btn
    >
  </v-app-bar>
</template>
<script>
export default {
  props: {
    foundUser: {
      type: Object,
      default: () => {}
    }
  }
};
</script>
