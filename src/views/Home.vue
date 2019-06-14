<template>
  <div>
  <StreamSub
    title="MLB Streams"
    link="https://www.reddit.com/r/mlbstreams/.json"
  ></StreamSub>
  <StreamSub
    title="NBA Streams"
    link="https://www.reddit.com/r/nbastreams/.json"
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
