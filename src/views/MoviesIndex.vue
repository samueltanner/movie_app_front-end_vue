<template>
  <div class="movies-index">
    <div class="row">
      <div class="col-sm-4" v-for="movie in movies" v-bind:key="movie.id">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">{{ movie.title }}</h5>
            <p class="card-text">{{ movie.year }}</p>
            <router-link v-bind:to="`/movies/${movie.id}`">
              <a href="#" class="btn btn-primary">More Info</a>
            </router-link>
          </div>
        </div>
      </div>
    </div>

    <div v-for="movie in movies" v-bind:key="movie.id">
      <router-link v-bind:to="`/movies/${movie.id}`">
        <h1>{{ movie.title }}</h1>
      </router-link>
      <h3>{{ movie.year }}</h3>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      movies: [],
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("/api/movies").then((response) => {
        console.log(response.data);
        this.movies = response.data;
      });
    },
  },
};
</script>
