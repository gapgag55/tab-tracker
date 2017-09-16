<template>
  <v-layout>
    <v-flex xs4>
      <panel title="Song Metadata">
        <v-text-field
          label="Title"
          v-model="song.title"
          required
          :rules="[required]"
        ></v-text-field>

        <v-text-field
          label="Artist"
          v-model="song.artist"
          required
          :rules="[required]"
        ></v-text-field>

        <v-text-field
          label="Genre"
          v-model="song.genre"
          required
          :rules="[required]"
        ></v-text-field>

        <v-text-field
          label="Album"
          v-model="song.album"
          required
          :rules="[required]"
        ></v-text-field>

        <v-text-field
          label="Album Image Url"
          v-model="song.albumImageUrl"
          required
          :rules="[required]"
        ></v-text-field>

        <v-text-field
          label="Youtube ID"
          v-model="song.youtubeId"
          required
          :rules="[required]"
        ></v-text-field>
      </panel>
    </v-flex>

    <v-flex xs8>
      <panel title="Song Structure" class="ml-3">
        <v-text-field
          label="Tab"
          multi-line
          v-model="song.tab"
          required
          :rules="[required]"
        ></v-text-field>

        <v-text-field
          label="Lyrics"
          multi-line
          v-model="song.lyrics"
          required
          :rules="[required]"
        ></v-text-field>

      </panel>

      <div class="danger-alert" v-if="error">
        {{error}}
      </div>

      <v-btn 
        class="deep-purple darken-4"
        @click="save">
        Update Song
      </v-btn>

    </v-flex>
  </v-layout>
</template>

<script>
import SongsService from '@/services/SongsService'

export default {
  data () {
    return {
      song: {
        title: null,
        artist: null,
        genre: null,
        album: null,
        albumImageUrl: null,
        youtubeId: null,
        lyrics: null,
        tab: null
      },
      error: null,
      required: (value) => !!value || 'Required.'
    }
  },
  methods: {
    async save () {
      const areAllFieldsFilledIn = Object
        .keys(this.song)
        .every(key => !!this.song[key])
      if (!areAllFieldsFilledIn) {
        this.error = 'Please fill in all the required fields.'
        return
      }
      try {
        await SongsService.put(this.song)
        this.$router.push({
          name: 'songs',
          params: {
            songId: this.song.id
          }
        })
      } catch (err) {
        console.log(err)
      }
    }
  },
  async mounted () {
    try {
      const songId = this.$store.state.route.params.songId
      this.song = (await SongsService.show(songId)).data
      console.log(this.song)
    } catch (err) {
      console.log(err)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
