<template>
  <Loader v-if="$fetchState.pending" />
  <div v-else class="container single-movie">
    <button @click="goBack" class="button">Go back</button>
    <div class="movie-info">
      <div class="movie-poster">
        <img
          :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
          :alt="movie.original_title"
        />
      </div>
      <div class="movie-content">
        <h1>{{ movie.title }}</h1>
        <p class="movie-fact tagline" v-if="movie.tagline !== ''">
          <span>Tagline:</span> "{{ movie.tagline }}"
        </p>
        <p class="movie-fact">
          <span>Genres:</span>
          <b v-for="genre in movie.genres" :key="genre.id">
            {{ genre.name }},</b
          >
        </p>
        <p class="movie-fact">
          <span>Status:</span>
          {{ movie.status }}
        </p>
        <p class="movie-fact">
          <span>Release Date:</span>
          {{
            new Date(movie.release_date).toLocaleString("en-us", {
              month: "long",
              day: "numeric",
              year: "numeric",
            })
          }}
        </p>
        <p class="movie-fact">
          <span>Duration:</span>
          {{ movie.runtime }} minutes
        </p>
        <p class="movie-fact">
          <span>Revenue:</span>
          {{
            movie.revenue.toLocaleString("en-us", {
              style: "currency",
              currency: "USD",
            })
          }}
        </p>
        <p class="movie-fact">
          <span>Overview:</span>
          {{ movie.overview }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "SingleMovie",
  head() {
    return {
      title: `${this.movie.title} - Movie App: Get Info about 20 most popular movies of the year.`,
    };
  },
  data() {
    return {
      movie: {},
    };
  },
  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=8170643a54bc2417dee997d1249b2f88&languge=en-US`
      );
      const result = await data;
      this.movie = result.data;
      console.log(this.movie);
    },
    goBack() {
      this.$router.go(-1);
    },
  },
  fetchDelay: 1000,
  async fetch() {
    await this.getSingleMovie();
  },
};
</script>

<style lang="scss" scoped>
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;

  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }

  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;

    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }

    .movie-poster {
      img {
        max-height: 500px;
        width: 100%;

        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }

    .movie-content {
      h1 {
        font-size: 40px;
        font-weight: 400;
      }

      .movie-fact {
        margin-top: 12px;
        font-size: 18px;
        line-height: 1.5;

        span {
          font-weight: 600;
          text-decoration: underline;
          display: inline-block;
        }
      }

      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>