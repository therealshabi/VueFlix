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
    //   movies :  {
    //     "dunkirk": {
    //         "id": 'dunkirk',
    //         "title": "Dunkirk",
    //         "subtitle": "Dunkirk",
    //         "description": `Miraculous evacuation of Allied soldiers from Belgium, Britain, Canada, and France, who were cut off and surrounded by the German army from the beaches and harbor of Dunkirk, France, during the Battle of France in World War II.`,
    //         "largeImgSrc": `url('https://image.tmdb.org/t/p/w780/fudEG1VUWuOqleXv6NwCExK0VLy.jpg')`,
    //         "smallImgSrc": 'https://image.tmdb.org/t/p/w185/fudEG1VUWuOqleXv6NwCExK0VLy.jpg',
    //         "releaseDate": 'July 21 2017',
    //         "duration": '1hr 46min',
    //         "genre": 'Action, Drama, History',
    //         "trailerPath": 'https://www.youtube.com/embed/F-eMt3SrfFU',
    //         "favorite": false
    //     },
    //     "interstellar": {
    //         "id": 'interstellar',
    //         "title": "Interstellar",
    //         "subtitle": "Interstellar",
    //         "description": `Interstellar chronicles the adventures of a group of explorers who make use of a newly discovered wormhole to surpass the limitations on human space travel and conquer the vast distances involved in an interstellar voyage.`,
    //         "largeImgSrc": `url('https://image.tmdb.org/t/p/w780/xu9zaAevzQ5nnrsXN6JcahLnG4i.jpg')`,
    //         "smallImgSrc": 'https://image.tmdb.org/t/p/w185/xu9zaAevzQ5nnrsXN6JcahLnG4i.jpg',
    //         "releaseDate": 'November 7 2014',
    //         "duration": '2hr 49min',
    //         "genre": 'Adventure, Drama',
    //         "trailerPath": 'https://www.youtube.com/embed/zSWdZVtXT7E',
    //         "favorite": false
    //     },
    //     "the-dark-knight-rises": {
    //         "id": 'the-dark-knight-rises',
    //         "title": "The Dark Knight Rises",
    //         "subtitle": "TDKR",
    //         "description": `Batman encounters the mysterious Selina Kyle and the villainous Bane, a new terrorist leader who overwhelms Gotham's finest. The Dark Knight resurfaces to protect a city that has branded him an enemy.`,
    //         "smallImgSrc": 'https://image.tmdb.org/t/p/w185/3bgtUfKQKNi3nJsAB5URpP2wdRt.jpg',
    //         "largeImgSrc": `url('https://image.tmdb.org/t/p/w780/3bgtUfKQKNi3nJsAB5URpP2wdRt.jpg')`,
    //         "releaseDate": 'July 20 2012',
    //         "duration": '2hr 44min',
    //         "genre": 'Action, Thriller',
    //         "trailerPath": 'https://www.youtube.com/embed/g8evyE9TuYk',
    //         "favorite": false
    //     },
    //     "inception": {
    //         "id": 'inception',
    //         "title": "Inception",
    //         "subtitle": "Inception",
    //         "description": `Cobb, a skilled thief is offered a chance to regain his old life as payment for a task considered to be impossible: \"inception\", the implantation of another person's idea into a target's subconscious.`,
    //         "smallImgSrc": 'https://image.tmdb.org/t/p/w185/s2bT29y0ngXxxu2IA8AOzzXTRhd.jpg',
    //         "largeImgSrc": `url('https://image.tmdb.org/t/p/w780/s2bT29y0ngXxxu2IA8AOzzXTRhd.jpg')`,
    //         "releaseDate": 'July 10 2010',
    //         "duration": '2hr 28min',
    //         "genre": 'Action, Adventure, Sci-Fi',
    //         "trailerPath": 'https://www.youtube.com/embed/8hP9D6kZseM',
    //         "favorite": false
    //     },
    //     "the-prestige": {
    //         "id": 'the-prestige',
    //         "title": "The Prestige",
    //         "subtitle": "Prestige",
    //         "description": `A mysterious story of two magicians whose intense rivalry leads them on a life-long battle for supremacy - to create the ultimate illusion whilst sacrificing everything they have to outwit the other.`,
    //         "smallImgSrc": 'https://image.tmdb.org/t/p/w185/c5o7FN2vzI7xlU6IF1y64mgcH9E.jpg',
    //         "largeImgSrc": `url('https://image.tmdb.org/t/p/w780/c5o7FN2vzI7xlU6IF1y64mgcH9E.jpg')`,
    //         "releaseDate": 'October 20 2006',
    //         "duration": '2hr 10min',
    //         "genre": 'Drama, Mystery, Sci-Fi',
    //         "trailerPath": 'https://www.youtube.com/embed/ijXruSzfGEc',
    //         "favorite": false
    //     }
// }
  }),
  created () {
        const API_KEY = "ab5a7db6d911744f7e4e910d9eb5582c";
        const BASE_URL = "https://api.themoviedb.org/3/discover/movie";
        const BASE_IMAGE_URL = "https://image.tmdb.org/t/p/w300_and_h450_bestv2";
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
                    largeImgSrc: BASE_IMAGE_URL+movie.poster_path,
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
    /* .row {
        display: flex;
        flex-direction: row;
        justify-content: space-around;
    } */

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
