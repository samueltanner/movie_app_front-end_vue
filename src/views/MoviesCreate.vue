<template>
  <div class="container movies-create">
    <form action="movie-create" method="post" v-on:submit.prevent="submitMovie()">
      <h1>Add Movie</h1>

      <div>
        <ul>
          <li class="text danger" v-for="error in errors" v-bind:key="error">{{ error }}</li>
        </ul>
      </div>

      <span class="form-group">
        <label for="title">Movie Title:</label>
        <input class="from-controll" type="text" id="title" name="title" size="50" v-model="title" required />
      </span>
      <br />
      <span class="form-group">
        <label for="year">Year:</label>
        <input
          class="from-control"
          type="number"
          id="year"
          name="year"
          min="1900"
          placeholder="Release Year"
          required
          v-model="year"
        />
      </span>
      <br />
      <span class="form-group">
        <label for="director">Director:</label>
        <input class="from-control" type="text" id="director" name="director" required v-model="director" />
      </span>
      <br />
      <span class="form-group">
        <label for="plot">Plot:</label>
        <textarea
          class="from-control"
          id="plot"
          name="plot"
          rows="5"
          cols="50"
          placeholder="Once upon a time..."
          required
          v-model="plot"
        />
        <small>{{ 300 - plot.length }} Characters Remaining</small>
      </span>
      <br />
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
          required
          v-model="image_url"
        />
      </span>
      <br />
      <button type="reset">Reset</button>
      <button type="submit" class="btn btn-primary" value="Submit">Add Movie to IMdB</button>

      <!-- <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="title"/>
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
      <input type="submit" class="btn btn-primary" value="Submit" /> -->
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
