<template>
  <div class="layout-wrapper" :style="{ height: scrollerHeight }">
    <div class="lay-title">布鲁记账</div>
    <div class="content" :class="classPrefix && `${classPrefix}-content`" :style="{ 'max-height': contentHeight }">
      <slot></slot>
    </div>
    <Nav></Nav>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";
@Component
export default class App extends Vue {
  @Prop(String) readonly classPrefix: string | undefined;
  get scrollerHeight() {
    return document.documentElement.clientHeight + "px";
  }
  get contentHeight() {
    const contentHeight = document.documentElement.clientHeight - 54;
    return contentHeight + "px";
  }
}
</script>

<style scoped lang="scss">
.lay-title {
  text-align: center;
  line-height: 30px;
  height: 30px;
  background: skyblue;
  border: 1px solid #aaa;
  border-radius: 10px;
  //   padding:6px;
}
.layout-wrapper {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.layout-wrapper::-webkit-scrollbar {
  display: none;
}

.content {
  flex-grow: 1; // 让内容尽量占满
  overflow: auto;
  // display: flex;
  // flex-direction: column-reverse;
}
.money-content {
  display: flex;
  flex-direction: column-reverse;
}
</style>
