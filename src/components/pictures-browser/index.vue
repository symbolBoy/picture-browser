<template>
  <div class="pictures-browser">
    <!-- 图片浏览器分为三个部分 上中下 -->
    <div class="top">
      <div class="close-btn" @click="handleCloseBrowser">
        <Close></Close>
      </div>
    </div>
    <div class="center">
      <div class="controls">
        <div class="prev" @click="handlePrev">
          <ArrowLeft></ArrowLeft>
        </div>
        <div class="next" @click="handleNext">
          <ArrowRight></ArrowRight>
        </div>
      </div>
      <div class="picture">
       
      </div>
    </div>
    <div class="bottom">
      <div class="preview">
        <div class="desc">
          <div class="left">
            共{{ currentIndex + 1 }} / {{ props.pictures.length }}, 第{{
              currentIndex + 1
            }}张图片
          </div>
          <div class="right" @click="changeIsShowPictureList">
            {{ isShowPictureList ? "隐藏" : "显示" }}图片列表
          </div>
        </div>
        <div
          class="preview-list"
          :style="{ height: isShowPictureList ? '67px' : '0px' }"
        >
          <Indicator :index="currentIndex">
            <div
              class="preview-item"
              v-for="(item, index) in pictures"
              :key="index"
              :class="{ active: currentIndex === index }"
              @click="setCurrentIndex(index)"
            >
              <img :src="item" alt="" />
            </div>
          </Indicator>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onBeforeUnmount, onMounted, onUpdated, ref } from "vue";
import Close from "../../assets/svg/close.vue";
import ArrowLeft from "../../assets/svg/arrow-left.vue";
import ArrowRight from "../../assets/svg/arrow-right.vue";
import Indicator from "../../components/indicator/index.vue";

onMounted(() => {
  window.document.body.style.overflow = "hidden";
});
onBeforeUnmount(() => {
  window.document.body.style.overflow = "auto";
});
// 定一个自定义事件名称
let $emit = defineEmits(["closeBrowser"]);

const props = defineProps({
  pictures: {
    type: Array,
    default: [],
  },
});

const isNext = ref(false);
const currentIndex = ref(0);
const isShowPictureList = ref(true);

// 隐藏掉图片浏览器
function handleCloseBrowser() {
  $emit("closeBrowser");
}

// 切换上一张图片
function handlePrev() {
  isNext.value = false;
  currentIndex.value = currentIndex.value - 1;
  // 如果左边越界了，就直接跳到最后一张图片
  if (currentIndex.value < 0) {
    currentIndex.value = props.pictures.length - 1;
  }
}
// 切换下一张图片
function handleNext() {
  isNext.value = true;
  currentIndex.value = currentIndex.value + 1;
  // 如果右边越界了，就直接跳到第一张图片
  if (currentIndex.value > props.pictures.length - 1) {
    currentIndex.value = 0;
  }
}
// 是否展示图片预览列表
function changeIsShowPictureList() {
  isShowPictureList.value = !isShowPictureList.value;
}

// 切换index
function setCurrentIndex(index) {
  // 如果切换的索引比当前的索引大，那么我们将isNext状态设置为true，设置下一张图片需要用到的动画
  if (index > currentIndex.value) {
    isNext.value = true;
  }
  // 如果切换的索引比当前的索引小，那么我们将isNext状态设置为false，设置上一张图片需要用到的动画
  if (index < currentIndex.value) {
    isNext.value = false;
  }
  currentIndex.value = index;
}
</script>

<style scoped lang="scss">
.pic-enter-from {
  opacity: 0;
  transform: translateX(100%);
}
.pic-enter-active {
  transition: transform 0.2s ease, opacity 0.2s ease;
}

.pic-leave-active {
  transition: opacity 0.2s ease;
}

.pic-enter-to,
.pic-leave-from {
  opacity: 1;
  transform: translateX(0);
}

.pic-leave-to {
  opacity: 0;
  transform: translateX(0);
}

.pic2-enter-from {
  opacity: 0;
  transform: translateX(-100%);
}
.pic2-enter-active {
  transition: transform 0.2s ease, opacity 0.2s ease;
}

.pic2-leave-active {
  transition: opacity 0.2s ease;
}

.pic2-enter-to,
.pic2-leave-from {
  opacity: 1;
  transform: translateX(0);
}

.pic2-leave-to {
  opacity: 0;
  transform: translateX(0);
}
.pictures-browser {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background: #333;
  display: flex;
  flex-direction: column;
  /* 顶部区域 关闭按钮 */
  .top {
    position: relative;
    width: 100%;
    height: 80px;
    .close-btn {
      position: absolute;
      top: 10px;
      right: 20px;
      color: red;
    }
  }

  /* 中间区域 展示图片 */
  .center {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    flex: 1;
    color: #fff;
    .controls {
      position: absolute;
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: space-between;
      .prev,
      .next {
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 9;
      }
    }
    .picture {
      position: relative;
      height: 100%;
      width: 100%;
      max-width: 105vh;
      overflow: hidden;
      img {
        position: absolute;
        left: 0;
        right: 0;
        top: 0;
        margin: 0 auto;
        height: 100%;
        user-select: none;
        /* object-fit: cover; */
      }
    }
  }

  /* 底部图片 指示器 */
  .bottom {
    display: flex;
    justify-content: center;
    margin-top: 10px;
    color: #fff;
    height: 100px;
    .preview {
      position: absolute;
      bottom: 10px;
      max-width: 105vh;
      .desc {
        display: flex;
        justify-content: space-between;
        .right {
          cursor: pointer;
        }
      }
      .preview-list {
        margin-top: 3px;
        overflow: hidden;
        transition: height 300ms ease;
        .preview-item {
          margin-right: 5px;
          cursor: pointer;
          img {
            height: 67px;
            opacity: 0.5;
          }
          &.active {
            img {
              opacity: 1;
            }
          }
        }
      }
    }
  }
}
</style>
