<template>
  <div class="container movies-edit">
    <form v-on:submit.prevent="updateMovie(movie)">
      <h1>Edit Movie</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <input type="text" class="form-control" v-model="movie.title" />
      <input type="text" class="form-control" v-model="movie.year" />
      <input type="text" class="form-control" v-model="movie.plot" />
      <input type="text" class="form-control" v-model="movie.director" />
      <input type="text" class="form-control" v-model="movie.image_url" />
      <br />
      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
    <button v-on:click="destroyMovie(movie)">Delete Movie</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      movie: {},
      errors: [],
    };
  },
  created: function () {
    axios.get("/api/movies/" + this.$route.params.id).then((response) => {
      console.log(response.data);
      this.movie = response.data;
    });
  },
  methods: {
    updateMovie: function (movie) {
      var params = {
        title: movie.title,
        director: movie.director,
        plot: movie.plot,
        year: movie.year,
        image_url: movie.image_url,
      };
      axios
        .patch("/api/movies/" + movie.id, params)
        .then((response) => {
          console.log("movie was updated", response.data);
          this.$router.push("/movies/" + this.movie.id);
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = error.response.data.errors;
        });
    },
    destroyMovie: function (movie) {
      axios.delete("/api/movies/" + movie.id).then(() => {
        console.log("movie was destroyed");
        this.$router.push("/movies/");
      });
    },
  },
};
</script>
