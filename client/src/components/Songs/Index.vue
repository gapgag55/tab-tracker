<template>
  <v-layout>
    <v-flex xs6 v-if="isUserLoggedIn">
      <songs-bookmarks/>
      <recently-viewed-song class="mt-2" />
    </v-flex>
    <v-flex :class="{xs12: !isUserLoggedIn, xs6: isUserLoggedIn}" class="ml-2">
      <songs-search-panel />
      <songs-panel class="mt-2"/>
    </v-flex>
  </v-layout>
</template>

<script>
import SongsPanel from './SongsPanel'
import SongsSearchPanel from './SongsSearchPanel'
import SongsBookmarks from './SongsBookmarks'
import RecentlyViewedSong from './RecentlyViewedSong'
import {mapState} from 'vuex'

export default {
  computed: {
    ...mapState(['isUserLoggedIn'])
  },
  components: {
    SongsPanel,
    SongsSearchPanel,
    SongsBookmarks,
    RecentlyViewedSong
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
