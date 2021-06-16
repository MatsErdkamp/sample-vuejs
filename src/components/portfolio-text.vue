<template>
  <div>
    <transition name="fade">
      <div v-if="detailViewOpened">
        <div class="detail-view">
          <div class="detail-view-title">{{ object["title"] }}</div>
          <div :key="snippet" v-for="snippet in details">
            <detail-snippet />
          </div>
          <detail-video />
        </div>
        <div class="detail-view-close">
          <img
            class="detail-view-close-icon"
            @click="detailViewOpened = false"
            src="@/assets/EXIT_ICON.svg"
            alt=""
          />
        </div>
      </div>
    </transition>

    <div class="text-title" :class="{ name: index == 0 }">{{ object["title"] }}</div>
    <div v-if="object['subtitle'] != undefined" class="text-subtitle">
      {{ object["subtitle"] }}
    </div>
    <div v-if="object['text'] != undefined" class="text-body">
      {{ object["text"] }}
    </div>
    <div class="text-buttons">
      <div
        v-if="index == 0"
        class="text-button"
        @click="this.$emit('clicked', index + 1)"
      >
        Take the tour →
      </div>
      <div
        v-if="index > 0"
        class="text-button"
        @click="this.$emit('clicked', index - 1)"
      >
        ← Go back
      </div>
      <div
        v-if="index > 0 && object['read_more'] == true"
        class="text-button-more"
        @click="detailViewOpened = true"
        style="margin-left: 24px"
      >
        Read more
      </div>
      <div
        v-if="index > 0"
        class="text-button"
        @click="this.$emit('clicked', index + 1)"
        style="margin-left: 24px"
      >
        Continue →
      </div>

      <!-- <div class="text-button" style="margin-left: 24px">Contact me</div> -->
    </div>
  </div>
</template>

<script>
import detailSnippet from "./portfolio-detail-snippet.vue";
import detailVideo from "./portfolio-detail-video.vue";

export default {
  components: {
    detailSnippet: detailSnippet,
    detailVideo: detailVideo
  },
  props: ["object", "index"],
  data: function() {
    return {
      detailViewOpened: false,
      details: [0, 1, 2, 3, 4]
    };
  },
  watch: {
    detailViewOpened: function(newval, oldval) {
      this.$emit("detailOpen", newval);
    }
  }
};
</script>

<style>

.name {
  font-size: 6em !important;
}

.detail-view {
  z-index: 10000;
  overflow-y: scroll;
  position: fixed;
  top: 0;
  left: 0;
  min-width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(12px);
  padding: 48px;
}

.detail-view-title {
  font-size: 4em;
  text-align: center;
  font-weight: 500;
}

.detail-view-close {
  z-index: 999999;
  position: absolute;
  right: 64px;
  top: 48px;
}

.detail-view-close-icon {
  width: 64px;
  cursor: pointer;
}
</style>