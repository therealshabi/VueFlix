<template>
  <div
    :style="{backgroundImage: `url(${movie.largeImgSrc})`}"
    :class="{'films-bg': true, favorite: movie.favorite}"
  >
    <div class="content">
      <div v-if="play" class="iframe-loader">
        <iframe :src="trailerPath" style="width:100%; height:100%" frameborder="0" allowfullscreen></iframe>
      </div>

      <div style="width:100%" v-else>
        <div class="header">
          <i
            class="material-icons"
            style="font-size:48px;color:red;cursor: pointer; margin:auto 0"
          >menu</i>
          <div class="nav">
            <div
              v-for="tab in tabs"
              v-bind:key="tab"
              :class="{selected: selectedTab==tab}"
              v-on:click="handleTab(tab)"
            >{{tab}}</div>
          </div>
          <div
            style="color:red;margin: auto 10px;font-weight:bold;cursor: pointer;"
            v-on:click="goHome"
          >VUEFLIX</div>
        </div>

        <div style="height:100%; width:100%; display:flex; flex-direction:row;">
          <div
            style="text-align:left;margin: 5px 40px; line-height:1.2; width:65%; justfy-content:space-around"
          >
            <p>
              <a
                style="color:white; font-size:40px; font-weight:bold"
                :href="homepage"
                id="title"
                target="_blank"
              >{{movie.title}}</a>
              <br />
              <span
                style="color:red; font-weight:bold"
              >{{this.runtime}} | {{this.genre}} | {{movie.releaseDate}}</span>
            </p>

            <p style="color:white;line-height:1.5" class="fixed-line">{{movie.description}}</p>

            <div style="display:flex">
              <div
                style="display:flex; cursor:pointer; background-color:#DE0000;color:white; border-radius: 500px; border:none; width:fit-content; padding:5px 12px 5px 16px"
                v-on:click="fetchYoutubeURL"
              >
                <span style="display:block;margin-top:3px;font-weight:bold;margin-right:2px">Play</span>
                <i class="material-icons" style="font-size:24p; margin:auto 0">play_arrow</i>
              </div>

              <div
                v-if="movie.favorite"
                class="fav"
                style="display:flex;cursor:pointer"
                v-on:click="toggleFav"
              >
                <span style="margin:4px">Remove from favorites</span>
                <i
                  class="material-icons"
                  style="font-size:18px;margin: auto 4px;"
                >remove_circle_outline</i>
              </div>

              <div v-else class="fav" style="display:flex;cursor:pointer" v-on:click="toggleFav">
                <span style="margin:4px">Add to favorites</span>
                <i
                  class="material-icons"
                  style="font-size:18px;margin: auto 4px;"
                >add_circle_outline</i>
              </div>
            </div>
          </div>

          <div style="height:280px; width:40%">
            <img :src="movie.smallImgSrc" style="max-height:100%; max-width:100%" class="img-box" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import _ from 'lodash';

export default {
  name: 'Films',
  props: {
    movie: {
      type: Object,
      required: true
    },

    play: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      selectedTab: 'Films',
      genre: '',
      runtime: '',
      homepage: '',
      BASE_YOUTUBE_TRAILER_URL: 'https://www.youtube.com/embed/',
      API_KEY: 'ab5a7db6d911744f7e4e910d9eb5582c',
      BASE_URL: 'https://api.themoviedb.org/3/movie/',
      trailerPath: '',
      tabs: ['Home', 'Films', 'Shows', 'Music']
    };
  },
  methods: {
    toggleFav() {
      this.$emit('toggleFavorite', this.movie.id);
      this.$forceUpdate();
    },

    handleTab(tab) {
      this.selectedTab = tab;
      if (tab == 'Home') {
        this.goHome();
      }
    },

    fetchYoutubeURL() {
      this.$emit('togglePlay');
      axios
        .get(this.BASE_URL + this.movie.id + '/videos', {
          params: {
            api_key: this.API_KEY,
            language: 'en-US'
          }
        })
        .then(response => {
          this.trailerPath =
            this.BASE_YOUTUBE_TRAILER_URL + response.data.results[0].key;
        });
    },

    goHome() {
      this.$emit('goHome');
    }
  },

  updated() {
    const API_KEY = 'ab5a7db6d911744f7e4e910d9eb5582c';
    const BASE_URL = 'https://api.themoviedb.org/3/movie/';

    axios
      .get(BASE_URL + this.movie.id, {
        params: {
          api_key: API_KEY,
          language: 'en-US'
        }
      })
      .then(response => {
        let x = [];
        let duration = response.data.runtime;
        let hour = Math.floor(duration / 60);
        let min = duration % 60;
        this.runtime = hour + 'hr ' + min + 'min';
        this.homepage = response.data.homepage;
        _.forEach(response.data.genres, (genre, index) => {
          x.push(genre.name);
          if (index > 1) {
            return false;
          }
        });
        this.genre = x.join(',');
      });
  },

  created() {
    const API_KEY = 'ab5a7db6d911744f7e4e910d9eb5582c';
    const BASE_URL = 'https://api.themoviedb.org/3/movie/';

    axios
      .get(BASE_URL + this.movie.id, {
        params: {
          api_key: API_KEY,
          language: 'en-US'
        }
      })
      .then(response => {
        let x = [];
        let duration = response.data.runtime;
        let hour = Math.floor(duration / 60);
        let min = duration % 60;
        this.runtime = hour + 'hr ' + min + 'min';
        this.homepage = response.data.homepage;
        _.forEach(response.data.genres, (genre, index) => {
          x.push(genre.name);
          if (index > 1) {
            return false;
          }
        });
        this.genre = x.join(',');
      });
    '';
  }
};
</script>

<style>
html,
body {
  margin: 0px;
  height: 100%;
}
</style>

<style scoped>
.col {
  display: flex;
  flex-direction: column;
  height: 100%;
  justify-content: space-between;
}

.header {
  display: flex;
  width: 100%;
  padding: 6px;
  justify-content: flex-start;
  align-items: flex-start;
  box-sizing: border-box;
}

.content {
  height: 100%;
  width: 100%;
  background-image: radial-gradient(
    circle at 20% 50%,
    rgba(16.47%, 15.29%, 14.51%, 0.5) 0%,
    rgba(16.47%, 15.29%, 14.51%, 0.88) 100%
  );
}

.films-bg {
  position: relative;
  background-repeat: no-repeat;
  background-size: cover;
  /* Important for applying gradient to background images*/
  background-color: rgba(0, 0, 0, 0.7);
  background-blend-mode: multiply;
  width: 100%;
  height: 100%;
  padding: 0px;
}

.nav {
  display: flex;
  color: white;
  flex-grow: 3;
  margin: auto 0;
}

.nav div {
  padding: 4px 16px;
  cursor: pointer;
}

.nav .selected {
  background: white;
  color: black;
  border-radius: 500px;
}

.fav {
  color: white;
  margin: auto 40px;
}

.favorite {
  box-shadow: 0px 0px 70px #cc8e33;
}

.fixed-line {
  overflow: hidden;
  text-overflow: ellipsis;
  max-height: 6em;
}

.iframe-loader {
  background: url(../assets/images/loading.gif) center center no-repeat;
  background-size: 85px;
  width: 100%;
  height: 100%;
}

.img-box {
  box-shadow: 0px 0px 100px 20px #000000;
}

#title:hover {
  cursor: pointer;
  color: #0cbbff !important;
}

a {
  color: white;
  text-decoration: none;
}
</style>
