<script lang="ts" setup>
defineProps(["item"]);
</script>

<template>
  <div class="gallery-item">
    <img
      :src="item.url"
      @error="(event) => $emit('handleLoadImageError', event, item)"
      alt=""
    />
    <span>{{ item.label }}</span>
  </div>
</template>

<style lang="scss" scoped>
.gallery-item {
  flex: 0 0 auto;
  width: 160px;
  height: 140px;
  // border: 1px solid #000000;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 5px;

  img {
    max-width: 100%;
    border-radius: 5px;
  }

  span {
    font-family: serif;
    font-size: 12px;
    color: #000;

    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
}
</style>
