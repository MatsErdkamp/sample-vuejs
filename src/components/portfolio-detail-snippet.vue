<template>
  <div class="snippet-parent">
    <img class="snippet-image" src="" alt="" />
    <div class="snippet-text-parent">
      <div class="snippet-title">title</div>
      <div class="snippet-subtitle">subtitle</div>
      <div class="snippet-text">
        text and more text and more a lot of text text text text should be a lot
        I guess
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style>
.snippet-parent {
  display: flex;
  margin: 20px;
  width: 100%;
  align-items: center;
}

.snippet-text-parent {
  padding: 0 24px;
}

.snippet-title {
  font-size: 2.4em;
  font-weight: 500;
  line-height: 1.1;
}

.snippet-subtitle {
  font-size: 1em;
  color: grey;
}

.snippet-text {
  font-size: 1em;
}

.snippet-image {
  width: 20vw;
  background: grey;
  height: 20vw;
}
</style>