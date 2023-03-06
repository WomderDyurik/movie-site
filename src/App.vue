<template>
  <div id="app">
    <MovieNotification />
    <PreLoader />
    <PosterBg :poster="posterBg" />
    <MovieHeader />
    <MoviesList :list="moviesList" @changePoster="onChangePoster" />
    <MoviesPagination
      :current-page="currentPage"
      :per-page="moviesPerPage"
      :total="moviesLength"
      @pageChanged="onPageChanged"
    />
  </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
import MoviesList from "@/components/moviesList.vue";
import PosterBg from "@/components/PosterBg.vue";
import MoviesPagination from "@/components/MoviesPagination.vue";
import PreLoader from "@/components/PreLoader.vue";
import MovieHeader from "@/components/MovieHeader.vue";
import MovieNotification from "@/components/Notification.vue";
export default {
  name: "app",
  components: {
    MovieHeader,
    MoviesList,
    PosterBg,
    MoviesPagination,
    PreLoader,
    MovieNotification,
  },
  data: () => ({
    posterBg: "",
  }),
  computed: {
    ...mapGetters("movies", [
      "moviesList",
      "currentPage",
      "moviesPerPage",
      "moviesLength",
    ]),
  },
  watch: {
    "$route.query": {
      handler: "onPageQueryChange",
      immediate: true,
      deep: true,
    },
  },
  methods: {
    ...mapActions("movies", ["changeCurrentPage"]),
    onPageQueryChange({ page = 1 }) {
      this.changeCurrentPage(Number(page));
    },
    onChangePoster(poster) {
      this.posterBg = poster;
    },
    onPageChanged(page) {
      this.$router.push({ query: { page } });
      this.changeCurrentPage(page);
    },
  },
  created() {
    if (this.$route.query.page) {
      this.changeCurrentPage(Number(this.$route.query.page));
    }
  },
};
</script>

<style>
#app {
  font-family: Arial, Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  position: relative;
  min-height: 100vh;
}
</style>
