<template>
  <div>
    <div class="snippet-parent" v-if="snippet.conclusion == undefined">
      <img class="snippet-image" v-bind:src="findSource()" />
      <div class="snippet-text-parent">
        <div class="snippet-title">{{ snippet.title }}</div>

        <div class="snippet-text">
          <p>{{ snippet.text }}</p>
          <p v-if="snippet.text_2 != undefined">
            {{ snippet.text_2 }}
          </p>
        </div>
      </div>
    </div>
    <div class="snippet-parent-center" v-else>
      <div class="snippet-title-center">
        {{ snippet.title }}
      </div>
      <p class="snippet-text-center">{{ snippet.text }}</p>
      <p v-if="snippet.text_2 != undefined" class="snippet-text-center">{{ snippet.text_2 }}</p>
      <p v-if="snippet.text_3 != undefined" class="snippet-text-center">{{ snippet.text_3 }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["snippet"],
  methods: {
    findSource: function() {
      var string = this.snippet.image;

      return string;
    }
  }
};
</script>

<style>
.snippet-parent {
  display: flex;
  margin: 20px;
  min-width: 100%;
  align-items: center;
}

.snippet-parent-center {
  margin-top: 48px;
  min-width: 100%;
  text-align: center;
}

.snippet-text-parent {
  padding: 0 24px;
}

.snippet-title {
  font-size: 2.4em;
  font-weight: 500;
  line-height: 1.4;
  margin-bottom: 12px;
}

.snippet-title-center {
  font-size: 2.4em;
  font-weight: 500;
  line-height: 1.4;

  margin-bottom: 12px;
  text-align: center;
}

.snippet-subtitle {
  font-size: 1em;
  color: grey;
}

.snippet-text {
  font-size: 1em;
}

.snippet-text-center {
  font-size: 1em;
  display: block;
  width: 50%;
  margin: auto;
  text-align: justify;
}

.snippet-image {
  border-radius: 4px;
  width: 20vw;
  background: grey;
  height: 20vw;
}
</style>