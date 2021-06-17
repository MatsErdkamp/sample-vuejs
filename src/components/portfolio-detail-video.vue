<template>
  <div v-if="this.index == 3" class="snippet-video-parent">
    <iframe
      :src="getSource()"
      width="720"
      height="480"
      frameborder="0"
      allow="autoplay; fullscreen; picture-in-picture"
      allowfullscreen
    ></iframe>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      videoAvailable: false
    };
  },
  props: ["object", "index"],
  methods: {
    getSource: function() {
      



      return "https://player.vimeo.com/video/557989484";
    }
  }
};
</script>

<style>
.snippet-video-parent {
  display: flex;
  justify-content: center;
  width: 100%;
  align-items: center;
  margin-bottom: 64px;
}
</style>