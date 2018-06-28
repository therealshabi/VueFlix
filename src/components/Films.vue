<template>
<div :style='{backgroundImage: movie.largeImgSrc}' :class="{'films-bg': true, favorite: movie.favorite}">
    <iframe :src="movie.trailerPath" style="width:100%; height:100%" frameborder="0" allowfullscreen v-if="play"></iframe>
    <div style="width:100%" v-else> 
        <div class="header">
            <i class="material-icons" style="font-size:48px;color:red;cursor: pointer; margin:auto 0">menu</i>
            <div class="nav">
                <div v-for="tab in tabs" :class="{selected: selectedTab==tab}" v-on:click="handleTab(tab)">{{tab}}</div>
            </div>
            <div style="color:red;margin: auto 10px;font-weight:bold;cursor: pointer;" v-on:click="goHome">VUEFLIX</div>
        </div>

        <div style="text-align:left;margin: 5px 40px; line-height:1.2; width:45%">
            <p>
                <span style="color:white; font-size:40px; font-weight:bold">{{movie.title}}</span><br>
                <span style="color:red; font-weight:bold">{{movie.duration}} | {{movie.genre}} | {{movie.releaseDate}}</span>
            </p>
            <p style="color:white;line-height:1.5" class=fixed-line>{{movie.description}}</p>
            <div style="display:flex">
                <div style="display:flex; cursor:pointer; background-color:#DE0000;color:white; border-radius: 500px; border:none; width:fit-content; padding:5px 12px 5px 16px" v-on:click="$emit('togglePlay')">
                    <span style="display:block;margin-top:3px;font-weight:bold;margin-right:2px">Play</span> 
                    <i class="material-icons" style="font-size:24p; margin:auto 0">play_arrow</i>
                </div>
                <div v-if="movie.favorite" class="fav" style="display:flex;cursor:pointer" v-on:click="toggleFav">
                    <span style="margin:4px">Remove from favorites</span>
                    <i class="material-icons" style="font-size:18px;margin: auto 4px;">remove_circle_outline</i>
                </div>
                <div v-else class="fav" style="display:flex;cursor:pointer" v-on:click="toggleFav">
                    <span style="margin:4px">Add to favorites</span>
                    <i class="material-icons" style="font-size:18px;margin: auto 4px;">add_circle_outline</i>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
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
      },
  },
  data() {
      return {
          selectedTab: "Films",
          tabs: [
              "Home",
              "Films",
              "Shows",
              "Music"
          ],
      };
  },
  methods: {
      toggleFav() {
          this.$emit('toggleFavorite',this.movie.id);
      },

      handleTab(tab) {
        this.selectedTab=tab;
        if (tab=="Home") {
            this.goHome();
        }
      },

      goHome() {
          this.$emit('goHome');
      }
  }
}
</script>

<style>
    html, body {
        margin: 0px;
        height:100%;
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
    } 

    .header {
        display: flex;
        width:100%;
        padding: 6px;
        justify-content: flex-start;
        align-items: flex-start; 
        box-sizing: border-box;
    }

    .films-bg {
        position: relative;
        background-repeat: no-repeat;
        background-size:cover;
        /* Important for applying gradient to background images*/
        background-color: rgba(0, 0, 0, 0.7);
        background-blend-mode: multiply;
        padding:0px;
    }

    .nav {
        display:flex;
        color: white;
        flex-grow:3;
        margin: auto 0;
    }

    .nav div {
        padding: 4px 16px;
        cursor: pointer;
    }

    .nav .selected {
        background: white;
        color: black;
        border-radius:500px;
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
</style>
