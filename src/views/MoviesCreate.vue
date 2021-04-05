<template>
  <div class="container movies-create">
    <form v-on:submit.prevent="submitMovie()">
      <h1>Add Movie</h1>
      <div>
        <ul>
          <li class="text danger" v-for="error in errors" v-bind:key="error">{{ error }}</li>
        </ul>
      </div>

      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="title" />
      </div>
      <div class="form-group">
        <label>Year:</label>
        <input type="text" class="form-control" v-model="year" />
      </div>
      <div class="form-group">
        <label>Plot:</label>
        <input type="text" class="form-control" v-model="plot" />
        <small>{{ 300 - plot.length }} Characters Remaining</small>
      </div>
      <div class="form-group">
        <label>Director:</label>
        <input type="text" class="form-control" v-model="director" />
      </div>
      <div class="form-group">
        <label>Poster URL:</label>
        <input type="text" class="form-control" v-model="image_url" />
      </div>
      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      title: "",
      year: "",
      plot: "",
      director: "",
      image_url: "",
      errors: [],
    };
  },
  methods: {
    submitMovie: function () {
      var params = {
        title: this.title,
        year: this.year,
        plot: this.plot,
        director: this.director,
        image_url: this.image_url,
      };
      axios
        .post("/api/movies/", params)
        .then((response) => {
          console.log("Movie being added", response.data);
          this.$router.push("/movies");
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
