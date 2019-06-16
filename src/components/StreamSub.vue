<template>
  <div>
    <div class="home">
      <button
        class="btn btn-primary"
        type="button"
        data-toggle="collapse"
        v-bind:data-target="`#` + toggle"
        aria-expanded="false"
        aria-controls="collapseExample"
        @click="clearLinks()"
      >
        {{ title }}
      </button>

      <div class="collapse" v-bind:id="toggle">
        <div class="card card-body">
          <div v-for="streamPost in streamPosts">
            <button class="btn btn-primary">
              <a @click="scrapeInfoFromStreamPost(streamPost.data.url)">{{
                streamPost.data.title
              }}</a>
            </button>
            <div v-if="click">
              <GameLink v-bind:links="links"></GameLink>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import GameLink from "@/components/GameLink.vue";

export default {
  components: {
    GameLink
  },

  props: ["link", "title", "toggle"],

  data() {
    return {
      streamPosts: null,
      comments: [],
      num_comments: null,
      errors: null,
      click: false,
      titles: [],
      links: []
    };
  },

  mounted: function() {
    // Gets posts from r/nba --> Need to implement only game thread posts
    axios.get(this.link).then(response => {
      // JSON responses are automatically parsed.

      // streams contains post-level information
      this.streamPosts = response.data.data.children;

      // Filters out streamPosts that do not contain 'Game'
      this.streamPosts = this.streamPosts.filter(post => {
        console.log(this.streamPosts);
        return post.data.title.includes("Game");
      });

      // console.log(this.streamPosts);

      // Gets individual post URLs and used to access comments containg stream links
      this.getURLs(this.streamPosts);
    });
    // .catch(e => {
    //   this.errors.push(e);
    //   console.log(this.errors);
    // });
  },

  methods: {
    //Getters
    //--------------------------------------------------------
    getURLs: function(streamPosts) {
      this.streamURLs = streamPosts.map(streamPost => {
        return streamPost.data.url;
      });
    },

    // Collects body of each top-level comment where stream link is stored
    getStreamsFromPost: function(fromPostStreams) {
      // Regex to collect links for top level comments on stream posts
      const linkCheck = /href=(["'])(.*?)\1/gm;

      // Clears links array so that links from associated post display when clicked
      this.links = [];

      // Extracts links from comments in stream posts
      for (let i = 0; i < fromPostStreams.length; i++) {
        let matches = fromPostStreams[i].data.body_html.matchAll(linkCheck);
        for (let match of matches) {
          // console.log(match[2]);
          this.links.push(match[2]);
        }
      }

      // Filters link array of any discord or internal reddit links
      this.links = this.links.filter(this.filterLinks);

      // console.log(this.links);

      // All top comment level information into comments object
      this.comments = fromPostStreams.map(fromPostStream => {
        return fromPostStream.data;
      });
    },

    //--------------------------------------------------------

    //Post Scrapers
    //--------------------------------------------------------
    scrapeInfoFromStreamPost: function(URL) {
      // console.log(URL);

      this.click = false;

      axios.get(URL + `.json`).then(response => {
        this.click = true;

        // console.log(this.click);

        // Collects body of each top-level comment where stream link is stored
        this.getStreamsFromPost(response.data[1].data.children);

        // This is a comment object
        //console.log(response.data[1].data.children[0].data);
      });
      //--------------------------------------------------------
    },

    filterLinks: function(link) {
      if (
        link.includes("http") &&
        !link.includes("discord") &&
        !link.includes("reddit")
      ) {
        return true;
      }
    },

    clearLinks: function() {
      this.links = [];
    }
  }
};
</script>
