<template>
  <div class="movies-index">
    <p>
      Search by title:
      <input v-model="titleFilter" list="titles" />
    </p>
    <datalist id="titles">
      <option v-for="movie in movies" v-bind:key="movie.id">{{ movie.title }}</option>
    </datalist>
    <div>
      <button>Sort Alphabetically</button>
    </div>
    <div class="row">
      <div
        class="col-sm-4"
        v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), 'title')"
        v-bind:key="movie.id"
      >
        <div class="card">
          <div class="card-body">
            <div class="card-image">
              <img v-bind:src="movie.image_url" class="card-img-top" v-bind-alt="movie.title" />
            </div>
            <h5 class="card-title">{{ movie.title }}</h5>
            <p class="card-text">{{ movie.year }}</p>
            <router-link v-bind:to="`/movies/${movie.id}`">
              <a href="#" class="btn btn-primary">More Info</a>
            </router-link>
          </div>
        </div>
      </div>

      <!-- <div v-for="movie in movies" v-bind:key="movie.id">
      <router-link v-bind:to="`/movies/${movie.id}`">
        <h1>{{ movie.title }}</h1>
      </router-link>
      <h3>{{ movie.year }}</h3>
    </div> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      movies: [],
      titleFilter: "",
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
