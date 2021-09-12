<template>
  <div class="hero">
    <img src="~/assets/images/movieHero.jpg" alt="Hero Cover Image" />
    <div class="text-container">
      <div class="text">
        <span class="small-heading"> Now Streaming </span>
        <h1><span>Now</span> Streaming</h1>
        <a href="#movies-grid" class="button">View Movies</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Hero",
};
</script>

<style lang="scss" scoped>
.hero {
  position: relative;
  height: 600px;

  @media screen and (max-width: 750px) {
    height: 700px;
  }

  &::after {
    position: absolute;
    content: "";
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.6);
  }

  img {
    height: 100%;
    width: 100%;
    object-fit: cover;
  }

  .text-container {
    position: absolute;
    z-index: 1;
    top: 0;
    margin: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;

    .text {
      padding: 0 16px;
      width: 100%;
      max-width: 1400px;
      margin: 0 auto;

      .small-heading {
        font-weight: 600;
        font-size: 18px;
        text-transform: uppercase;
        margin-bottom: 20px;
        color: #c92502;

        @media (min-width: 750px) {
          font-size: 22px;
        }
      }

      h1 {
        color: #fff;
        font-weight: 200;
        font-size: 64px;
        margin-bottom: 20px;

        @media (min-width: 750px) {
          font-size: 84px;
        }

        span {
          font-weight: 500;
        }
      }

      .button {
        font-size: 20px;
      }
    }
  }
}
</style>