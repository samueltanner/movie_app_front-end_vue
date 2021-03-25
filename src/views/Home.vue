<template>
  <div class="home">
    <h1>WELCOME TO THE MOVIES</h1>

    <div>
      <h1>Add a Movie</h1>
      <div id="movieInputArea">
        <h5>Movie Title:</h5>
        <input type="text" v-model="newMovieTitle" />
        <h5>Movie Year:</h5>
        <input type="text" v-model="newMovieYear" />
        <h5>Movie Director:</h5>
        <input type="text" v-model="newMovieDirector" />
        <h5>Movie Plot:</h5>
        <input type="text" v-model="newMoviePlot" />
      </div>

      <button v-on:click="addMovie()">Add Movie</button>
    </div>
    <br />

    <div v-for="movie in movies" v-bind:key="movie.id">
      <h3>{{ movie.title }} - {{ movie.year }}</h3>
      <button v-on:click="showMovieInfo(movie)">More Info</button>
      <!-- <button v-on:click="updateMovie(movie)">Update Movie</button> -->
      <div></div>
    </div>

    <dialog id="movie-info">
      <form method="dialog">
        <h1>Update Movie Info</h1>
        <p>
          Title:
          <input type="text" v-model="currentMovie.title" placeholder="Input Movie Title" />
        </p>
        <p>
          Year:
          <input type="text" v-model="currentMovie.year" placeholder="Input Movie Year" />
        </p>
        <p>
          Plot:
          <input type="text" v-model="currentMovie.plot" placeholder="Input Movie Plot" />
        </p>
        <p>
          Director:
          <input type="text" v-model="currentMovie.director" placeholder="Input Movie Director" />
        </p>
        <button v-on:click="updateMovie(currentMovie)">Save Update</button>
        <button v-on:click="destroyMovie(currentMovie)">Delete Movie</button>
        <button>Close</button>
        <!-- <div v-for="genre in genres" v-bind="movie.id">
          <p>Genres: {{ currentMovie.genre }}</p>
        </div> -->
      </form>
    </dialog>
  </div>
</template>
<style>
#movieInputArea {
  justify-content: center;
  text-align: center;
  display: flex;
  align-items: baseline;
}

#movieInputArea,
input {
  margin: 10px;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      movies: [],
      showInfo: false,

      newMovieTitle: "",
      newMovieYear: "",
      newMovieDirector: "",
      newMoviePlot: "",
      currentMovie: {},
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("/api/movies").then((response) => {
        this.movies = response.data;
        console.log(response.data);
      });
    },
    addMovie: function () {
      console.log("adding a movie");
      var params = {
        director: this.newMovieDirector,
        plot: this.newMoviePlot,
        title: this.newMovieTitle,
        year: parseInt(this.newMovieYear),
      };
      axios
        .post("/api/movies", params)
        .then((response) => {
          console.log("Movie was added", response.data);
          this.movies.push(response.data);
        })
        .catch(function (error) {
          console.log(error.response);
        });
    },
    showMovieInfo: function (movie) {
      console.log(movie);
      this.currentMovie = movie;
      document.querySelector("#movie-info").showModal();
    },
    // updateMovie: function (movie) {
    // console.log(movie);
    // this.currentMovie = movie;
    // document.querySelector("#update-movie").showModal();
    // },
    updateMovie: function (movie) {
      console.log("Updating Movie");
      var params = {
        title: movie.title,
        year: movie.year,
        plot: movie.plot,
        director: movie.director,
      };
      axios
        .patch(`/api/movies/${movie.id}`, params)
        .then((response) => {
          console.log("movie was updated", response.data);
        })
        .catch(function (error) {
          console.log(error.response);
        });
    },
    destroyMovie: function (movie) {
      axios.delete("/api/movies/" + movie.id).then((response) => {
        console.log("movie deleted", response.data);
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    },
  },
};
</script>
