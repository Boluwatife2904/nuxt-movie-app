<template>
  <div class="movie">
    <div class="movie-poster">
      <img
        :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
        :alt="movie.original_title"
      />
      <p class="review">{{ movie.vote_average }}</p>
      <p class="overview">{{ movie.overview }}</p>
    </div>
    <div class="movie-info">
      <p class="title">{{ movie.title }}</p>
      <p class="release">
        Released:
        {{
          new Date(movie.release_date).toLocaleString("en-us", {
            month: "long",
            day: "numeric",
            year: "numeric",
          })
        }}
      </p>
      <nuxt-link
        :to="{ name: 'movies-movieid', params: { movieid: movie.id } }"
        class="button button-light"
        >Get more info</nuxt-link
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "Movie",
  props: {
    movie: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.movie {
  position: relative;
  display: flex;
  flex-direction: column;

  .movie-poster {
    position: relative;
    overflow: hidden;

    &:hover {
      .overview {
        transform: translateY(0);
      }
    }

    img {
      display: block;
      width: 100%;
      height: 100%;
      max-width: 100%;
    }

    .review {
      position: absolute;
      top: 0;
      left: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      width: 40px;
      height: 40px;
      background-color: #c92502;
      color: #fff;
      border-radius: 0 0 16px 0;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
        0 2px 4px -1px rgba(0, 0, 0, 0.06);
    }

    .overview {
      line-height: 1.5;
      position: absolute;
      bottom: 0;
      background-color: rgba(201, 38, 2, 0.9);
      padding: 12px;
      color: #fff;
      transform: translateY(100%);
      transition: 0.3s ease-in-out all;
    }
  }

  .movie-info {
    margin-top: auto;

    .title {
      margin-top: 8px;
      color: #fff;
      font-size: 20px;
      overflow: hidden;
      display: -webkit-box;
      -webkit-line-clamp: 1;
      -webkit-box-orient: vertical;
    }

    .release {
      margin-top: 8px;
      color: #c9c9c9;
    }

    .button {
      margin-top: 8px;
    }
  }
}
</style>