<template>
  <div>
    <!-- Need to pass the click event into StreamSub somehow so that game links are displayed per game thread-->

    <StreamSub
      title="MLB Streams"
      link="https://www.reddit.com/r/mlbstreams/.json"
      toggle="collapseMLB"
    ></StreamSub>
    <StreamSub
      title="NBA Streams"
      link="https://www.reddit.com/r/nbastreams/.json"
      toggle="collapseNBA"
    ></StreamSub>
    <StreamSub
      v-on:click.native="test()"
      title="Test"
      link="https://www.reddit.com/r/TestStream/.json"
      toggle="collapseTest"
    ></StreamSub>
  </div>
</template>

<style></style>

<script>
import StreamSub from "@/components/StreamSub.vue";

export default {
  components: {
    StreamSub
  },

  methods: {
    test: function() {
      console.log("you clicked");
    }
  }
};
</script>
