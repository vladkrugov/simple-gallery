<script setup lang="ts">
import { ref } from "vue";
import { ITEMS_DATA_URL, MAX_CHECK_PERIOD } from "../constants";
import TheGalleryItem from "./TheGalleryItem.vue";
import noImage from "../assets/no-image.png";

type ItemsData = {
  url: string;
  label: string;
  checkPeriod?: number;
};

const items = ref([] as ItemsData[]);
const loading = ref(true);
const error = ref();

async function getData() {
  try {
    const res = await fetch(ITEMS_DATA_URL);

    loading.value = false;

    const data = await res.json();
    if (!res.ok) {
      const err = (data && data.message) || res.statusText;
      throw new Error(err);
    }

    items.value = data;

  } catch (err) {
    loading.value = false;
    error.value = err;
  }
}

getData();

function handleLoadImageError(event: Event, item: ItemsData) {
  const el = event.target as HTMLImageElement;

  el.src = noImage;

  if (!item.checkPeriod) item.checkPeriod = 1000;
  if (item.checkPeriod > MAX_CHECK_PERIOD) return;

  setTimeout(() => (el.src = item.url), item.checkPeriod);

  item.checkPeriod *= 2;
}
</script>

<template>
  <div class="gallery">
    <TheGalleryItem
      v-for="item in items"
      :item="item"
      :key="item.url"
      @handle-load-image-error="handleLoadImageError"
    />

    <h2 v-if="loading">Loading...</h2>

    <h3 v-if="error">{{ error }}</h3>
  </div>
</template>

<style lang="scss" scoped>
.gallery {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  justify-content: center;
}
</style>
