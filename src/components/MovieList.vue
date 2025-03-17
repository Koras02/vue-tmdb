<template>
  <div>
    <h1>Movie List</h1>
    <div class="movies">
      <div class="movie" v-for="movie in movies" :key="movie.id">
        <img :src="getPosterUrl(movie.poster_path)" alt="" />
        <h1>{{ movie.title }}</h1>
        <p>{{ movie.release_date }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      movies: [],
    };
  },
  created() {
    this.fetchMovies();
  },
  methods: {
    async fetchMovies() {
      try {
        const response = await axios.get(
          `https://api.themoviedb.org/3/movie/popular?api_key=${process.env.VUE_APP_TMDB_API_KEY}&language=ko-KR`
        );
        this.movies = response.data.results;
      } catch (error) {
        console.error("Movie List Error", error);
      }
    },
    getPosterUrl(posterPath) {
      if (!posterPath) {
        return "https://via.placholder.com/200x300?text=No+Image";
      }
      return `https://image.tmdb.org/t/p/w500${posterPath}`;
    },
  },
};
</script>

<style>
.movie-list {
  text-align: center;
}
.movies {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 2px;
}
.movie {
  margin: 5px;
}
img {
  width: 200px;
  height: 300px;
  border-radius: 10px;
}

h1 {
  font-size: 1.2em;
}
</style>
