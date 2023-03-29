<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class = "player">
        <h2 class = "song-title">{{current.title}} - <span>{{ current.artist }}</span></h2>
        <div class = "controls">
          <button class = "previous" @click = "previous">Previous</button>
          <button class = "play" v-if = "!isPlaying" @click = "play">Play</button>
          <button class = "pause" v-else @click = "pause">Pause</button>
          <button class = "next" @click = "next">Next</button>
        </div>
      </section>
      <section class = "playlist">
        <h3>The Playlist</h3>
        <button v-for = "song in songs" :key = "song.src" @click = "play(song)" :class = "(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "SONG1",
          artist: "ARTIST1",
          src: require("./assets/epic-1-minute-trailer-cue-cinematic-10905.mp3")
        },
        {
          title: "SONG2",
          artist: "ARTIST2",
          src: require("./assets/run-11239.mp3")
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;

        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this))
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    previous () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }

  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    this.player.play();
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: sans-serif;
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: rgb(41, 40, 40);
  color: white;
}
main {
  width: 100%;
  max-width: 760px;
  margin: 0 auto;
  padding: 25px;
}
.song-title {
  color: black;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}

.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: white;
  background-color: red;
}

.next, .previous {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: white;
  background-color: red;
}

.playlist {
  padding: 0px 30px;
}

.playlist h3 {
  color: black;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}

.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.playlist .song:hover{
  color: red;
}

.playlist .song.playing {
  color: white;
  background-image: linear-gradient(to right, rgb(163, 27, 27), rgb(255, 0, 0));
}

</style>
