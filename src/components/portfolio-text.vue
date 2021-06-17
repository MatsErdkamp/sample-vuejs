<template>
  <div :style="[index > 6 ? 'position: fixed; left: 25%; width: 50%;' : '']">
    <transition name="fade">
      <div v-if="detailViewOpened">
        <div class="detail-view">
          <div class="detail-view-title">{{ object["title"] }}</div>
          <detail-video :index="index" />
          <div :key="snippet" v-for="snippet in object.snippets">
            <detail-snippet :snippet="snippet" />
          </div>
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

    <div class="text-title" :class="{ name: index == 0 }">
      {{ object["title"] }}
    </div>
    <div v-if="object['subtitle'] != undefined" class="text-subtitle">
      {{ object["subtitle"] }}
    </div>
    <div class="text-body">
      <p v-if="object['text'] != undefined">{{ object["text"] }}</p>
      <p v-if="object['text_2'] != undefined">{{ object["text_2"] }}</p>
      <p v-if="object['text_3'] != undefined">{{ object["text_3"] }}</p>
      <p v-if="object['text_4'] != undefined">{{ object["text_4"] }}</p>
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
        v-if="index > 0 && index < 9"
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
  overflow-y: scroll;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 200;
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
  position: fixed;
  right: 64px;
  top: 48px;
}

.detail-view-close-icon {
  width: 64px;
  cursor: pointer;
}
</style>