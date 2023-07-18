<template>
  <div class="indicator">
    <div class="content" ref="contentRef">
      <slot></slot>
    </div>
  </div>
</template>

<script setup>
import { onMounted, onUpdated, ref } from "vue";
const props = defineProps({
  index: {
    type: Number,
    default: 0,
  },
});
const contentRef = ref();

onMounted(() => {
  watchIndexChange();
});

onUpdated(() => {
  watchIndexChange();
});

function watchIndexChange() {
  // 父元素的可视宽度
  const parentNodeWidth = contentRef.value.clientWidth;
  //   父元素的总宽度
  const parentNodeScrollWidth = contentRef.value.scrollWidth;
  //   子元素的宽度
  const childrenNodeWidth = contentRef.value.children[props.index].clientWidth;
  //   子元素偏移自己最近定位的父元素的偏移量
  const childrenNodeOffsetLeft =
    contentRef.value.children[props.index].offsetLeft;
  //   滚动宽度
  let rollingWidth =
    childrenNodeOffsetLeft + childrenNodeWidth * 0.5 - parentNodeWidth * 0.5;
  // 如果滚动宽度<0，则将它设置为0，也就是让它到0的时候不再滚动。
  if (rollingWidth < 0) {
    rollingWidth = 0;
  }
  // 如果滚动宽度<最大可滚动宽度，则将它设置为最大可滚动宽度，也就是让它到最大可滚动宽度的时候不再滚动。
  if (rollingWidth > parentNodeScrollWidth - parentNodeWidth) {
    rollingWidth = parentNodeScrollWidth - parentNodeWidth;
  }
  //   使用transform整个元素，并设置过渡效果
  contentRef.value.style.transform = `translate(${-rollingWidth}px)`;
}
</script>

<style lang="scss" scoped>
.indicator {
  overflow: hidden;
  .content {
    position: relative;
    display: flex;
    align-items: center;
    transition: transform 300ms ease;
    > * {
      flex-shrink: 0;
    }
  }
}
</style>
