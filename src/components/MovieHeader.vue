<template>
  <header class="header">
    <BNavbar type="dark" variant="light">
      <b-container>
        <b-navbar-brand href="#">MovieDB</b-navbar-brand>
        <b-nav-form>
          <b-form-input
            class="mr-sm-2 search-input"
            placeholder="Search"
            debounce="500"
            v-model="searchValue"
          ></b-form-input>
        </b-nav-form>
      </b-container>
    </BNavbar>
  </header>
</template>

<script>
import { mapActions } from "vuex";
export default {
  name: "MovieHeader",
  data: () => ({
    searchValue: "",
  }),
  watch: {
    searchValue: "onSearchValueChange",
  },
  methods: {
    ...mapActions("movies", [
      "searchMovies",
      "fetchMovies",
      "toggleSearchState",
    ]),
    onSearchValueChange(value) {
      if (value) {
        this.searchMovies(value);
        this.toggleSearchState(true);
      } else {
        this.fetchMovies();
        this.toggleSearchState(false);
      }
    },
  },
};
</script>

<style scoped>
.header {
  margin-bottom: 30px;
}
.navbar {
  background-color: rgba(0, 0, 0, 0.7) !important;
}
.search-input {
  color: #fff;
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(0, 0, 0, 0.6);
}
.search-input:focus {
  box-shadow: none;
  color: #fff;
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(0, 0, 0, 0.6);
}
</style>
