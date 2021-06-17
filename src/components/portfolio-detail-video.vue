<template>
  <div v-if="this.index == 3" class="snippet-video-parent">
    <iframe
      src="https://player.vimeo.com/video/557989484"
      width="720"
      height="405"
      frameborder="0"
      allow="autoplay; fullscreen; picture-in-picture"
      allowfullscreen
    ></iframe>
  </div>
  <div v-if="this.index == 4" class="snippet-video-parent">
    <iframe
      width="720"
      height="405"
      src="https://www.youtube.com/embed/Mr-g3E6nO8g"
      title="YouTube video player"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
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