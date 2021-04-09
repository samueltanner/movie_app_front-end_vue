<template>
  <div class="container movies-edit">
    <form v-on:submit.prevent="updateMovie(movie)">
      <h1>Edit Movie</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <span class="form-group">
        <label for="title">Movie Title:</label>
        <input class="from-controll" type="text" id="title" name="title" size="50" v-model="movie.title" />
      </span>
      <!-- <input type="text" class="form-control" v-model="movie.title" /> -->
      <span class="form-group">
        <label for="year">Year:</label>
        <input
          class="from-control"
          type="number"
          id="year"
          name="year"
          min="1900"
          placeholder="Release Year"
          v-model="movie.year"
        />
      </span>
      <!-- <input type="text" class="form-control" v-model="movie.year" /> -->
      <span class="form-group">
        <label for="director">Director:</label>
        <input class="from-control" type="text" id="director" name="director" v-model="movie.director" />
      </span>
      <!-- <input type="text" class="form-control" v-model="movie.director" /> -->
      <span>
        <textarea
          type="text"
          class="form-control"
          name="plot"
          rows="5"
          cols="50"
          placeholder="Once upon a time..."
          v-model="movie.plot"
        />
        <small>{{ 300 - movie.plot.length }} Characters Left</small>
        <span class="form-group">
          <label for="poster">Poster URL:</label>
          <input
            class="from-control"
            type="url"
            id="poster"
            name="image_url"
            placeholder="https://example.com"
            pattern="https://.*"
            size="50"
            v-model="movie.image_url"
          />
        </span>
      </span>
      <!-- <input type="text" class="form-control" v-model="movie.image_url" /> -->
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

<style>
label {
  display: block;
}

input,
label {
  margin: 5px 0;
}

button {
  margin: 5px 5px 5px 5px;
}

small {
  display: block;
}
</style>
