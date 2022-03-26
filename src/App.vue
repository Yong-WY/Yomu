<template>
  <div id="app">
    <header>
      <p class="p-1"><img
          src="./assets/img/logo.png"
          width="27px"
          height="27px"
        >尤姆</p>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist}}</span></h2>
        <div class="controls">
          <button
            class="prev"
            @click="prev"
          >&lt;</button>
          <button
            class="play"
            v-if="!isPlaying"
            @click="play"
          >播放</button>
          <button
            class="pause"
            v-else
            @click="pause"
          >暂停</button>
          <button
            class="next"
            @click="next"
          >&gt;</button>
        </div>
      </section>
      <section class="playlist">
        <h3>播放列表</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="(song.src == current.src) ? 'song playing' : 'song'"
        >
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Guns N Roses",
          artist: "G-Eazy & Post Malone",
          src: require("./assets/mp3/gunsNRoses.mp3"),
        },
        {
          title: "Far Alone",
          artist: "G-Eazy",
          src: require("./assets/mp3/farAlone.mp3"),
        },
        {
          title: "World Away",
          artist: "Lil Peep",
          src: require("./assets/mp3/worldAway.mp3"),
        },
        {
          title: "Plastic Dreams",
          artist: "G-Eazy (ft.Johanna Fay)",
          src: require("./assets/mp3/plasticDreams.mp3"),
        },
        {
          title: "Boss Tycoon",
          artist: "G-Eazy (ft.Johanna Fay)",
          src: require("./assets/mp3/bossTycoon.mp3"),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }

      this.player.play();
      this.player.addEventListener(
        "ended",
        function () {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }

          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

body {
  font-family: sans-serif;
  background-color: rgba(0, 0, 0, 0.849);
  background-position: center;
}

header {
  display: flex;
  padding: 15px;
  background-color: rgba(0, 0, 0, 0.3);
}

.p-1 {
  color: #515151;
  background-color: transparent;
  font-size: 27px;
  text-align: left;
  font-weight: 600;
  text-decoration: none;
  cursor: pointer;
}

main {
  margin: 0 auto;
  max-width: 500px;
  padding: 30px 40px;
}

.song-title {
  color: rgb(214, 21, 53);
  font-size: 20px;
  font-weight: 700;
  text-align: center;
}

.song-title span {
  font-weight: 600;
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

.play,
.pause {
  font-size: 13px;
  font-weight: 500;
  padding: 7px 17px;
  margin: 0px 15px;
  border-radius: 7px;
  color: rgba(250, 250, 250, 0.199);
  background-color: rgba(0, 0, 0, 0.253);
}

.next,
.prev {
  font-size: 13px;
  font-weight: 500;
  padding: 7px 17px;
  margin: 0px 15px;
  border-radius: 7px;
  color: rgba(250, 250, 250, 0.199);
  background-color: rgba(0, 0, 0, 0.253);
}

.playlist {
  padding: 30px 0;
  background-color: rgba(175, 175, 175, 0.123);
  border-radius: 10px;
}

.playlist h3 {
  color: #000000;
  font-size: 18px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}

.playlist .song {
  color: #01070ed0;
  display: block;
  width: 100%;
  padding: 15px 10px;
  font-size: 13px;
  font-weight: 500;
  cursor: pointer;
}

.playlist .song:hover {
  color: rgba(250, 250, 250, 0.199);
}

.playlist .song.playing {
  color: rgba(250, 250, 250, 0.199);
}
</style>
