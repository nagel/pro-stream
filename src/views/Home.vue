<template>
  <div>
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
</div>
</template>

<style></style>

<script>
import StreamSub from "@/components/StreamSub.vue";

export default {
  components: {
    StreamSub
  }
};
</script>
