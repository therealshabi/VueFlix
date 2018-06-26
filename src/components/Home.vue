<template>
  <div class="col">
      <div class="header" v-if="movieId==''">
          <span style="color:#FF0000;font-size:64px;">VUEFLIX</span>
          <span style="color:#fff; font-size:20px;">Select a movie below from the list of critically acclaimed <br>Christopher Nolan films.</span>
      </div>
      <films class= "header" v-else :movie="movies[movieId]" :play="play" @togglePlay="play=!play" @toggleFavorite="toggleFavorite" @goHome = "movieId=''"></films>
      <section id="footer">
          <div v-for="movie in movies" v-on:click="handleMovieClick(movie.id)" style="flex:20%;padding:0px 10px; position: relative">
            <img :src="movie.smallImgSrc" alt="placeholder" style="width:90%;">
            <i v-if="movie.favorite" class="material-icons" style="position:absolute; right:10%; top: 2px; color:yellow; font-size:18px" >check_circle</i>
          </div>
      </section>
  </div>
</template>

<script>
import Films from './Films'

export default {
  name: 'Home',
  components: {
      Films
  },
  data: () => ({
      movieId: "",
      play:false,
      movies :  {
        "dunkirk": {
            "id": 'dunkirk',
            "title": "Dunkirk",
            "subtitle": "Dunkirk",
            "description": `Miraculous evacuation of Allied soldiers from Belgium, Britain, Canada, and France, who were cut off and surrounded by the German army from the beaches and harbor of Dunkirk, France, during the Battle of France in World War II.`,
            "largeImgSrc": `url('https://image.tmdb.org/t/p/w780/fudEG1VUWuOqleXv6NwCExK0VLy.jpg')`,
            "smallImgSrc": 'https://image.tmdb.org/t/p/w185/fudEG1VUWuOqleXv6NwCExK0VLy.jpg',
            "releaseDate": 'July 21 2017',
            "duration": '1hr 46min',
            "genre": 'Action, Drama, History',
            "trailerPath": 'https://www.youtube.com/embed/F-eMt3SrfFU',
            "favorite": false
        },
        "interstellar": {
            "id": 'interstellar',
            "title": "Interstellar",
            "subtitle": "Interstellar",
            "description": `Interstellar chronicles the adventures of a group of explorers who make use of a newly discovered wormhole to surpass the limitations on human space travel and conquer the vast distances involved in an interstellar voyage.`,
            "largeImgSrc": `url('https://image.tmdb.org/t/p/w780/xu9zaAevzQ5nnrsXN6JcahLnG4i.jpg')`,
            "smallImgSrc": 'https://image.tmdb.org/t/p/w185/xu9zaAevzQ5nnrsXN6JcahLnG4i.jpg',
            "releaseDate": 'November 7 2014',
            "duration": '2hr 49min',
            "genre": 'Adventure, Drama',
            "trailerPath": 'https://www.youtube.com/embed/zSWdZVtXT7E',
            "favorite": false
        },
        "the-dark-knight-rises": {
            "id": 'the-dark-knight-rises',
            "title": "The Dark Knight Rises",
            "subtitle": "TDKR",
            "description": `Batman encounters the mysterious Selina Kyle and the villainous Bane, a new terrorist leader who overwhelms Gotham's finest. The Dark Knight resurfaces to protect a city that has branded him an enemy.`,
            "smallImgSrc": 'https://image.tmdb.org/t/p/w185/3bgtUfKQKNi3nJsAB5URpP2wdRt.jpg',
            "largeImgSrc": `url('https://image.tmdb.org/t/p/w780/3bgtUfKQKNi3nJsAB5URpP2wdRt.jpg')`,
            "releaseDate": 'July 20 2012',
            "duration": '2hr 44min',
            "genre": 'Action, Thriller',
            "trailerPath": 'https://www.youtube.com/embed/g8evyE9TuYk',
            "favorite": false
        },
        "inception": {
            "id": 'inception',
            "title": "Inception",
            "subtitle": "Inception",
            "description": `Cobb, a skilled thief is offered a chance to regain his old life as payment for a task considered to be impossible: \"inception\", the implantation of another person's idea into a target's subconscious.`,
            "smallImgSrc": 'https://image.tmdb.org/t/p/w185/s2bT29y0ngXxxu2IA8AOzzXTRhd.jpg',
            "largeImgSrc": `url('https://image.tmdb.org/t/p/w780/s2bT29y0ngXxxu2IA8AOzzXTRhd.jpg')`,
            "releaseDate": 'July 10 2010',
            "duration": '2hr 28min',
            "genre": 'Action, Adventure, Sci-Fi',
            "trailerPath": 'https://www.youtube.com/embed/8hP9D6kZseM',
            "favorite": false
        },
        "the-prestige": {
            "id": 'the-prestige',
            "title": "The Prestige",
            "subtitle": "Prestige",
            "description": `A mysterious story of two magicians whose intense rivalry leads them on a life-long battle for supremacy - to create the ultimate illusion whilst sacrificing everything they have to outwit the other.`,
            "smallImgSrc": 'https://image.tmdb.org/t/p/w185/c5o7FN2vzI7xlU6IF1y64mgcH9E.jpg',
            "largeImgSrc": `url('https://image.tmdb.org/t/p/w780/c5o7FN2vzI7xlU6IF1y64mgcH9E.jpg')`,
            "releaseDate": 'October 20 2006',
            "duration": '2hr 10min',
            "genre": 'Drama, Mystery, Sci-Fi',
            "trailerPath": 'https://www.youtube.com/embed/ijXruSzfGEc',
            "favorite": false
        }
}
  }),
  methods: {
      toggleFavorite(movieId) {
          this.movies[movieId].favorite  = !this.movies[movieId].favorite;
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
        flex: 28%;
        justify-content:space-around; 
        align-items:center;       
        background-color: #A3081E;
        z-index:2;
    }

    .header {
        display:flex;
        flex-direction: column;
        flex: 72%;
        background-color:#1F1D1D;
        align-items:center;
        justify-content:center;
    }
</style>
