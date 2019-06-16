<template>
  <div>
    <ul>
      <li v-for="link in links">
        <a v-bind:href="link" target="_blank">{{ link }}</a>
      </li>
    </ul>
  </div>
</template>

<script>
import GameLink from "@/components/GameLink.vue";

export default {
  props: ["links"],

  data() {
    return {};
  },

  mounted: function() {
  },

  methods: {}
};
</script>
