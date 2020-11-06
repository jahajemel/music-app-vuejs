<template>
  <div id="app">
    <header>
      <h1 class="bg-gray-700 text-white p-4 font-bold text-center text-xl">My Music</h1>
      
     <link href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css" rel="stylesheet">
    
    </header>
    <main class="flex flex-col justify-center text-center">
      <section class="player">
        <h2 class="song-title font-bold text-white text-2xl uppercase italic bg-gray-500 p-3">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="controls bg-gray-300 p-3">
          <button class="prev border border-gray-600 text-gray-700 rounded-md px-4 py-2 m-2 transition duration-500 ease select-none hover:bg-gray-100 focus:outline-none focus:shadow-outline" @click="prev">Prev</button>
          <button class="play border border-teal-500 text-teal-500 rounded-md px-4 py-2 m-2 transition duration-500 ease select-none hover:text-white hover:bg-teal-600 focus:outline-none focus:shadow-outline" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause border border-teal-500 text-teal-500 rounded-md px-4 py-2 m-2 transition duration-500 ease select-none hover:text-white hover:bg-teal-600 focus:outline-none focus:shadow-outline" v-else @click="pause">Pause</button>
          <button class="next border border-gray-600 text-gray-700 rounded-md px-4 py-2 m-2 transition duration-500 ease select-none hover:bg-gray-100 focus:outline-none focus:shadow-outline" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist flex flex-col">
        <h3 class="font-bold text-2xl p-3 bg-teal-100">The Playlist</h3>
        <button class="font-bold text-black p-1" v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'bg-gray-500' : 'bg-gray-300'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
       {
          title: 'Mockingbird',
          artist: 'Eminem',
          src: require('./assets/Eminem-Mockingbird.mp3')
        },
        {
          title: 'Six Feet Under',
          artist: 'Billie Eilish',
          src: require('./assets/BillieEilish-SixFeet.mp3')
        },
        {
          title: 'Scared to be lonley',
          artist: 'Dua Lipa & Martin Garrix',
          src: require('./assets/DuaLipa-Lonely.mp3')
        },
        {
          title: 'Grace',
          artist: 'Bebe Rexha',
          src: require('./assets/BebeRexha-Grace.mp3')
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
      }.bind(this));
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
    prev () {
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
  }
}
</script>
