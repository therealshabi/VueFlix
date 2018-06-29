<template>
  <div class="col">
      <div class="header" v-if="movieId==''">
          <span style="color:#FF0000;font-size:64px;">VUEFLIX</span>
          <span style="color:#fff; font-size:20px;">Select a movie below from the list of critically acclaimed <br>Christopher Nolan films.</span>
      </div>
      <films class= "header" v-else :movie="movies[movieId]" :play="play" @togglePlay="play=!play" @toggleFavorite="toggleFavorite" @goHome = "movieId=''"></films>
      <section id="footer">
          <div style="display: flex;">
            <div v-if="loading" style="color:white;font-size:24px">Loading...</div>
            <div v-else v-for="movie in movies" v-on:click="handleMovieClick(movie.id)" style="padding:5px 20px; position: relative">
                <img :src="movie.smallImgSrc" alt="placeholder" style="width: 100%; height: 100%;">
                <i v-if="movie.favorite" class="material-icons" style="position:absolute; right:10%; top: 2px; color:yellow; font-size:18px" >check_circle</i>
            </div>
            </div>
      </section>
  </div>
</template>

<script>
import Films from './Films'
import axios from 'axios'
import _ from 'lodash'

export default {
  name: 'Home',
  components: {
      Films
  },
  data: () => ({
      movieId: "",
      play:false,
      loading: true,
      movies: {},
  }),
  
  created () {
        const API_KEY = "ab5a7db6d911744f7e4e910d9eb5582c";
        const BASE_URL = "https://api.themoviedb.org/3/discover/movie";
        const BASE_IMAGE_URL = "https://image.tmdb.org/t/p/w300_and_h450_bestv2";
        const BASE_BACKDROP_IMAGE = "https://image.tmdb.org/t/p/w1400_and_h450_face";
        const BASE_YOUTUBE_TRAILER_URL = "https://www.youtube.com/watch?v=GTJr8OvyEVQ";

        axios.get(BASE_URL, {
            params : {
                api_key : API_KEY,
                language : "en-US",
                sort_by: "popularity.desc",
                include_adult: false,
                page: 1
            }

        }).then(response => {
            _.forEach(response.data.results, (movie, index) => {
                this.movies[movie.id] = {
                    id: movie.id,
                    title: movie.title,
                    description: movie.overview,
                    largeImgSrc: BASE_BACKDROP_IMAGE+movie.backdrop_path,
                    smallImgSrc: BASE_IMAGE_URL+movie.poster_path,
                    releaseDate: movie.release_date,
                    genre: movie.genre_ids,
                    favorite: false
                };
                if (index > 5) 
                    return false;
            });
                this.loading = false;
        });
  },
  methods: {
      toggleFavorite(movieId) {
          this.movies[movieId].favorite  = !this.movies[movieId].favorite;
          this.$forceUpdate();
      },

      handleMovieClick(movieId) {
          this.movieId = movieId;
          this.play = false;
      }
  }
}
</script>

<style>
    html, body {
        margin: 0px;
        height:100%;
        padding: 40px 100px;
        box-sizing: border-box;
        background: linear-gradient(to bottom right, #7f3737, #2a2122);
    }
</style>

<style scoped>

    .col {
        display: flex;
        flex-direction: column;
        height:100%;
        justify-content:space-between;
        box-shadow: 0px 0px 100px #000;
    }

    #footer {
        display:flex;
        flex-direction: column;
        height: 300px;
        justify-content:space-around; 
        align-items:center;       
        background-color: #A3081E;
        z-index:2;
    }

    .header {
        display:flex;
        height: 100%;
        flex-direction: column;
        background-color:#1F1D1D;
        align-items:center;
        justify-content:center;
    }
</style>
