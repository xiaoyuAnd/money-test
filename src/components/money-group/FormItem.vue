<template>
  <div :class="isNewTag === '+' ? 'notes-wrapper notes-wrapper-no-bg' : 'notes-wrapper '">
    <label class="notes">
      <span class="name">{{ fieldName }}</span>
      <template v-if="type === 'date'">
        <input :value="x(value)" @input="onValueChange2($event.target.value)" :type="type || 'text'" :placeholder="placeholder" v-if="isNewTag !== '+'" />
      </template>
      <template v-else>
        <input :value="value" @input="onValueChange2($event.target.value)" type="text" :placeholder="placeholder" v-if="isNewTag !== '+'" />
      </template>
      <input :value="value" @input="onValueChange($event.target.value)" @compositionstart="onCompositionStart" @compositionend="onCompositionEnd" @keydown="onkeydown" type="text" :placeholder="placeholder" maxlength="3" v-if="isNewTag === '+'" />
    </label>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";
// import eventBus from '@/bus.ts';
import dayjs from "dayjs";

@Component
export default class FormItem extends Vue {
  @Prop(String) fieldName: string | undefined;
  @Prop(String) type?: string;

  @Prop(String) output: string | undefined;
  @Prop(String) placeholder?: string;
  @Prop({ default: "" }) isNewTag: string | undefined;
  @Prop(String) isNumberPad: string | undefined;
  @Prop({ default: "" }) value: string | undefined;

  lock = false;

  onCompositionStart() {
    this.lock = true;
  }

  onCompositionEnd() {
    this.lock = false;
  }

  onkeydown() {
    this.lock = false;
  }

  x(isoString: string) {
    console.log(isoString);
    console.log(dayjs);
    console.log(dayjs(isoString).format("YYYY-MM-DD"));
    return dayjs(isoString).format("YYYY-MM-DD");
  }

  onValueChange(value: string) {
    if (!this.lock) {
      this.$emit("update:tagName", value);
    }
  }

  onValueChange2(value: string) {
    this.$emit("update:value", value);
  }
}
</script>

<style scoped lang="scss">
.notes {
  font-size: 14px;
  background: transparent;
  padding-left: 16px;
  padding-right: 10px;
  // border: 1px solid #333;
  border-radius: 10px;

  .name {
    padding-right: 16px;
  }

  input {
    height: 36px;
    flex-grow: 1;
    background: transparent;
    border: none;
    padding-right: 12px;
    width: 200px;
  }
}

.notes-wrapper {
  position: relative;
  background: #fff;
  border: 1px solid #aaa;
  border-radius: 10px;
  margin: 0 3px 2px 3px;
  padding: 5px;
  flex-shrink: 1;
}

.notes-wrapper-no-bg {
  background: white;
}
</style>
