<template>
  <div>
    <button @click="previewPictures">查看我的图片</button>
    <PicturesBrowser
      :pictures="pictures"
      v-if="isShowBrowser"
      @closeBrowser="handleCloseBrowser"
    />
  </div>
</template>

<script setup>
import { ref } from "vue";
import PicturesBrowser from "./components/pictures-browser/index.vue";
// 我想要预览的图片
const pictures = [
  "src/assets/img/11.webp",
  "src/assets/img/01.webp",
  "src/assets/img/02.webp",
  "src/assets/img/03.webp",
  "src/assets/img/04.webp",
  "src/assets/img/05.webp",
  "src/assets/img/06.webp",
  "src/assets/img/07.webp",
  "src/assets/img/08.webp",
  "src/assets/img/09.webp",
  "src/assets/img/10.webp",
  "src/assets/img/01.webp",
  "src/assets/img/02.webp",
  "src/assets/img/03.webp",
  "src/assets/img/04.webp",
  "src/assets/img/05.webp",
  "src/assets/img/06.webp",
  "src/assets/img/07.webp",
  "src/assets/img/08.webp",
  "src/assets/img/09.webp",
  "src/assets/img/10.webp",
  "src/assets/img/01.webp",
  "src/assets/img/02.webp",
  "src/assets/img/03.webp",
  "src/assets/img/04.webp",
  "src/assets/img/05.webp",
  "src/assets/img/06.webp",
  "src/assets/img/07.webp",
  "src/assets/img/08.webp",
  "src/assets/img/09.webp",
  "src/assets/img/10.webp",
  "src/assets/img/01.webp",
  "src/assets/img/02.webp",
  "src/assets/img/03.webp",
  "src/assets/img/04.webp",
  "src/assets/img/05.webp",
  "src/assets/img/06.webp",
  "src/assets/img/07.webp",
  "src/assets/img/08.webp",
  "src/assets/img/09.webp",
  "src/assets/img/10.webp",
];
const isShowBrowser = ref(false);

// 预览图片
function previewPictures() {
  isShowBrowser.value = true;
}
// 关闭预览图片
function handleCloseBrowser() {
  isShowBrowser.value = false;
}
</script>

<style scoped lang="scss">

</style>
