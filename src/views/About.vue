<template>
  <div class="about">
    <form action="movie-create" method="post">
      <span>
        <label for="title">Movie Title:</label>
        <input type="text" id="title" name="title" size="50" required />
      </span>
      <br />
      <span>
        <label for="year">Year:</label>
        <input type="number" id="year" name="year" min="1900" placeholder="Release Year" required />
      </span>
      <br />
      <span>
        <label for="director">Director:</label>
        <input type="text" id="director" name="director" required />
      </span>
      <br />
      <span>
        <label for="plot">Plot:</label>
        <textarea id="plot" name="plot" rows="5" cols="50" placeholder="Once upon a time..." required />
      </span>
      <br />
      <span>
        <label for="poster">Poster URL:</label>
        <input
          type="url"
          id="poster"
          name="image_url"
          placeholder="https://example.com"
          pattern="https://.*"
          size="50"
          required
        />
      </span>
      <br />
      <button type="reset">Reset</button>
      <button type="submit">Add Movie to IMdB</button>
    </form>
  </div>
</template>
