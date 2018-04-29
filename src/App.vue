<template>
  <div id="app">
    <connection-listener @update:connection-status="connectionUpdate($event)"/>
    <radio-player class="audio-player" :disabled="offline" audio-source="http://5.133.182.164:27062/;stream.mp3" :no-art="noArt" :now-playing="nowPlaying" :play.sync="playing"/>

    <last-played-list :last-played="lastPlayed"/>


    <div id="backgroundArt"><div class="art" :style="{'background-image': 'url('+(nowPlaying.art || noArt)+')'}"></div></div>
  </div>
</template>

<script>
import RadioPlayer from './components/RadioPlayer/RadioPlayer';
import ConnectionListener from './components/ConnectionListener';
import LastPlayedList from './components/LastPlayedList';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    RadioPlayer,
    ConnectionListener,
    LastPlayedList
  },
  data: function() {
    return {
      noArt: 'https://4vector.com/i/free-vector-cd-icon_101662_CD_icon.png',
      nowPlaying: {
        id: null,
        title: null,
        artist: null,
        art: null,
      },
      lastPlayed: [],
      wasPlaying: true,
      playing: true,
      offline: false,
    }
  },
  created: function() {
    this.refreshNowPlaying();

    setInterval(function() {this.refreshNowPlaying()}.bind(this), 10*1000);
  },
  methods: {
    refreshNowPlaying: function() {
      if(this.offline) return;

      let that = this;
      axios.get('http://www.aeiscte-iul.pt/radio/get_lastplayed.php?type=np&id='+this.nowPlaying.id).then(function(res) {
        if(typeof res.data   == 'undefined') return;
        if(typeof res.data.id == 'undefined') return;
        if(res.data.id == this.nowPlaying.id) return;

        this.nowPlaying = {
          id: res.data.id,
          title: res.data.title,
          artist: res.data.artist,
          art: res.data.art,
        }

        axios.get('http://www.aeiscte-iul.pt/radio/get_lastplayed.php?type=lp').then(function(res) {
          this.lastPlayed = res.data;
        }.bind(this)).catch(function(error) {
          console.error('Erro: '+error.message+'. Por favor, contacta a AEISCTE-IUL se este erro perturbar a tua navegação.');
        })

      }.bind(this)).catch(function(error) {
        console.error('Erro: '+error.message+'. Por favor, contacta a AEISCTE-IUL se este erro perturbar a tua navegação.');
      })
    },
    connectionUpdate: function(connected) {
      if(!connected) {
        this.wasPlaying = this.playing;
      }

      this.offline = !connected;

      if(this.wasPlaying) {
        if(connected) 
          this.playing = true;
        else
          this.playing = false;
      }
    }
  }
}
</script>

<style>
body, html, #app {
  margin: 0;
  width: 100%;
  height: 100%;
  color: white;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;

  display: flex;
  justify-content: center;
  align-items: center;
}
@keyframes backgroundBlur {
  from {
    filter: blur(30px);
  }
  to {
    filter: blur(10px);
  }
}
#backgroundArt {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: -1;
  background-color: #282828;
}
#backgroundArt .art {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;

  z-index: -1;
  background-size: cover;
  background-position: center center;

  opacity: 0.2;

  animation-duration: 2.5s;
  animation-name: backgroundBlur;
  animation-iteration-count: infinite;
  animation-direction: alternate;
  animation-timing-function: ease-in-out;
}
.audio-player {
  width: 500px;
}
</style>
