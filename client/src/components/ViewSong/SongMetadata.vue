<template>
   <panel title="Song Metada">
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
              name: 'song-edit', 
              params () {
                return {
                  songId: song.id
                }
              }
            }">
            Edit
          </v-btn>

          <v-btn 
            class="deep-purple darken-4"
            v-if="isUserLoggedIn && !bookmark"
            @click="setAsBookmark">
            Set As Bookmark
          </v-btn>

          <v-btn 
            class="deep-purple darken-4"
            v-if="isUserLoggedIn && bookmark"
            @click="unsetAsBookmark">
            Unset As Bookmark
          </v-btn>

        </v-flex>

        <v-flex xs6>
          <img class="album-image" :src="song.albumImageUrl" />
          <br>
          <div>{{song.album}}</div>
        </v-flex>
      </v-layout>
    </panel>
</template>

<script>
import {mapState} from 'vuex'
import BookmarkService from '@/services/BookmarkService'
export default {
  props: ['song'],
  data () {
    return {
      bookmark: null
    }
  },
  computed: {
    ...mapState([
      'isUserLoggedIn',
      'user'
    ])
  },
  watch: {
    async song () {
      if (!this.isUserLoggedIn) {
        return
      }
      const bookmarks = (await BookmarkService.index({
        songId: this.song.id
      })).data
      if (bookmarks.length) {
        this.bookmark = bookmarks[0]
      }
    }
  },
  methods: {
    async setAsBookmark () {
      try {
        this.bookmark = (await BookmarkService.post({
          songId: this.song.id
        })).data
      } catch (err) {
        console.log(err)
      }
    },
    async unsetAsBookmark () {
      try {
        await BookmarkService.delete(this.bookmark.id)
        this.bookmark = null
      } catch (err) {
        console.log(err)
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
