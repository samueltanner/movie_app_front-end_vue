<template>
  <div class="container movies-show">
    <div>
      <img v-bind:src="movie.image_url" v-bind:alt="movie.title" />
      <h1>{{ movie.title }}</h1>
      <h3>{{ movie.year }}</h3>
      <h3>{{ movie.director }}</h3>
      <p>{{ movie.plot }}</p>
    </div>
    <router-link v-bind:to="`/movies/${movie.id}/edit`">
      <button>Edit</button>
    </router-link>
  </div>
</template>

<script>
import axios from "axios";
// import axios from "axios";

export default {
  data: function () {
    return {
      movie: {},
    };
  },
  created: function () {
    this.showMovie();
  },
  methods: {
    showMovie: function () {
      axios.get("/api/movies/" + this.$route.params.id).then((response) => {
        console.log(response.data);
        this.movie = response.data;
      });
    },
  },
};
</script>
