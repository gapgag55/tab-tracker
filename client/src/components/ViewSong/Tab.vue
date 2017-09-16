<template>
  <panel title="Tab">
    <textarea
      readonly
      v-model="song.tab"
    ></textarea>
  </panel>
</template>

<script>
export default {
  props: ['song']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  textarea {
    width: 100%;
    outline: 0;
    font-family: monospace;
    height: 600px;
    overflow: auto;
    font-size: 13px;
  }
</style>
