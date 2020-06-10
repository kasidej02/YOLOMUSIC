<template>
  <div id="app">
    <b-container class="center">
      <b-card
        img-src="https://www.img.in.th/images/55959ccb9a6322900fde6645dcaa850c.png"
        img-alt="Image"
        img-top
        tag="article"
        style="max-width: 20rem;"
        class="mb-2"
      >
        <main>
          <section class="player">
            <h2 class="song-title">
              {{ current.title }} - <span> {{ current.artist }}</span>
            </h2>
            <div class="control">
              <div class="pr-2">
                <b-button
                  pill
                  variant="outline-primary"
                  class="prev"
                  @click="prev"
                >
                  <img src="./assets/media-step-backward-2x.png"
                /></b-button>
              </div>
              <b-button
                pill
                variant="warning"
                class="play"
                v-if="!isPlaying"
                @click="play"
                ><img src="./assets/media-play-2x.png"
              /></b-button>
              <b-button
                pill
                variant="warning"
                class="pause"
                v-else
                @click="pause"
                ><img src="./assets/media-pause-2x.png"
              /></b-button>
              <div class="pl-2">
                <b-button
                  pill
                  variant="outline-primary"
                  class="next"
                  @click="next"
                  ><img src="./assets/media-step-forward-2x.png"
                /></b-button>
              </div>
            </div>
          </section>

          <b-container>
            <section class="playlist">
              <h3>Playlist</h3>
              <b-button
                block
                pill
                variant="warning"
                v-for="song in songs"
                :key="song.src"
                @click="play(song)"
                :class="song.src == current.src ? 'song plaing' : 'song '"
              >
                {{ song.title }} - {{ song.artist }}
              </b-button>
            </section>
          </b-container>
        </main>
      </b-card>
    </b-container>
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
          title: "Let's cry",
          artist: "Dept",
          src: require("./assets/Let-cry.mp3"),
          pic:
            "https://i0.wp.com/dudeplace.co/wp-content/uploads/2020/04/Poster-Dept-Lets-cry-1.jpg?w=1000&ssl=1"
        },
        {
          title: "เรื่องจริง",
          artist: "Sin",
          src: require("./assets/true-story.mp3"),
          pic:
            "https://1.bp.blogspot.com/-aTkv9adn_IY/VLSdt2rbAjI/AAAAAAAASNg/ddQ3Bl7uruo/s1600/Sin.jpg"
        },
        {
          title: "คนคนนึง",
          artist: "THE WHITE HAIR CUT",
          src: require("./assets/คนคนนึง - THE WHITE HAIR CUT [ Official MV ].mp3")
        }
      ],
      player: new Audio()
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
        function() {
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
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Mitr&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Mitr", sans-serif;
}

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.song-title {
  color: #212121;
  font-size: 32px;
  font-weight: 700;
  text-align: center;
}

.song-title span {
  font-weight: 400;
  font-style: italic;
}

.control {
  display: flex;
  justify-content: center;
  padding: 30px 15px;
}

.playlist.song.playing {
  color: #fff;
}

.card {
  margin: 50px auto;
  float: none;
  margin-bottom: 10px;
  background: linear-gradient(#ffcc33, #ffcc99);
}
</style>
