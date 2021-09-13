<template>
  <div class="home">
    <!-- Hero Section -->
    <Hero />

    <!-- Search Bar -->
    <div class="container search">
      <input
        type="text"
        name="search"
        id="search"
        placeholder="Search for your favorite movies"
        v-model.lazy="search"
        autocomplete="off"
        @keyup.enter="$fetch"
      />
      <button
        v-show="search !== '' || searchedMovies.length > 0"
        class="button"
        @click="clearSearch"
      >
        Clear Search
      </button>
    </div>
    <!-- Loader -->
    <Loader v-if="$fetchState.pending" />

    <!-- Movies Section -->
    <div v-else class="container movies">
      <!-- When Users search for a movie -->
      <div v-if="search !== ''" id="movies-grid" class="movies-grid">
        <Movie v-for="movie in searchedMovies" :key="movie.id" :movie="movie" />
      </div>

      <!-- Now playing movies that is loaded on default -->
      <div v-else id="movies-grid" class="movies-grid">
        <Movie v-for="movie in movies" :key="movie.id" :movie="movie" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  head() {
    return {
      title: "Movie App: Get Info about 20 most popular movies of the year.",
      meta: [
        {
          hid: 'keywords',
          name: "keywords",
          content: "popular, movies, 20, top"
        }
      ]
    }
  },
  data() {
    return {
      search: "",
      movies: [],
      searchedMovies: [],
    };
  },
  methods: {
    async getMovies() {
      try {
        const data = await axios.get(
          "https://api.themoviedb.org/3/movie/now_playing?api_key=8170643a54bc2417dee997d1249b2f88&language=en-US&page=1"
        );
        const result = await data;
        result.data.results.forEach((movie) => {
          this.movies.push(movie);
        });
      } catch (error) {
        console.log(error);
      }
    },
    async searchMovies() {
      try {
        const data = axios.get(
          `https://api.themoviedb.org/3/search/movie/?api_key=8170643a54bc2417dee997d1249b2f88&languge=en-US&page=1&query=${this.search}"`
        );
        const result = await data;
        this.searchedMovies = result.data.results;
      } catch (error) {
        console.log(error);
      }
    },
    clearSearch() {
      this.search = "";
      this.searchedMovies = [];
    },
  },
  fetchDelay: 1000,
  async fetch() {
    if (this.search === "") {
      await this.getMovies();
      return;
    }
    await this.searchMovies();
  },
};
</script>

<style lang="scss" scoped>
.search {
  display: flex;
  padding: 32px 16px;

  input {
    width: 100%;
    max-width: 350px;
    padding: 12px;
    font-size: 14px;
    border: none;
    font: inherit;

    &:focus {
      outline: none;
    }
  }

  button {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }
}
.movies {
  padding: 32px 16px;

  .movies-grid {
    display: grid;
    column-gap: 32px;
    row-gap: 64px;
    grid-template-columns: 1fr;

    @media (min-width: 500px) {
      grid-template-columns: repeat(2, 1fr);
    }

    @media (min-width: 992px) {
      grid-template-columns: repeat(3, 1fr);
    }

    @media (min-width: 1200px) {
      grid-template-columns: repeat(4, 1fr);
    }
  }
}
</style>
