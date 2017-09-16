<template>
  <panel title="Songs">
    <div 
      class="song"
      v-for="song in songs" 
      :key="song.id">

      <v-layout>
        <v-flex xs6>
          <div class="song-title">
            {{song.title}}
          </div>
          <div class="song-artist">
            {{song.artist}}
          </div>
          <div class="song-genre">
            {{song.genre}}
          </div>

          <v-btn 
            class="deep-purple darken-4"
            :to="{
              name: 'song', 
              params: {
                songId: song.id
              }
            }">
            View
          </v-btn>

        </v-flex>

        <v-flex xs6>
          <img class="album-image" :src="song.albumImageUrl" />
        </v-flex>
      </v-layout>
    </div>
    <v-btn
      slot="action" 
      :to="{name: 'songs-create'}"
      class="deep-purple darken-2"
      medium
      absolute
      right
      middle
      fab>
      <v-icon>add</v-icon>
    </v-btn>
  </panel>
</template>

<script>
import SongsService from '@/services/SongsService'
export default {
  data () {
    return {
      songs: null
    }
  },
  async mounted () {
    // do a request to the backend for all the songs
    this.songs = (await SongsService.index()).data
  },
  watch: {
    '$route.query.search': {
      immediate: true,
      async handler (value) {
        this.songs = (await SongsService.index(value)).data
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .song {
    padding: 20px;
    height: 330px;
    overflow: hidden;
  }

  .song-title {
    font-size: 30px;
  }

  .song-artist {
    font-size: 24px;
  } 

  .song-genre {
    font-size: 18px;
  }

  .album-image {
    width: 70%;
    margin: 0 auto;
  }
</style>
