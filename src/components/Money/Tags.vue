<template>
  <div class="tags">
    <div class="new">
      <button @click="createTag">新增标签</button>
    </div>
    <ul class="current">
      <li
        v-for="tag in tagList"
        :key="tag.id"
        :class="{ selected: selectedTags.indexOf(tag) >= 0 }"
        @click="toggle(tag)"
      >
        {{ tag.name }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import TagHelper from "@/mixins/TagHelper";
import Vue from "vue";
import { mixins } from "vue-class-component";
import { Component } from "vue-property-decorator";

@Component
export default class Tags extends mixins(TagHelper) {
  selectedTags: string[] = []; //存放被选中的标签

  get tagList() {
    return this.$store.state.tagList;
  }

  created() {
    this.$store.commit("fetchTags");
  }
  //选中和取消选中标签
  toggle(tag: string) {
    const index = this.selectedTags.indexOf(tag);
    if (index >= 0) {
      //如果在 就取消选中
      this.selectedTags.splice(index, 1);
    } else {
      this.selectedTags.push(tag);
    }
    //这是哪个事件？
    this.$emit("update:value", this.selectedTags);
  }
  //money页面的新增标签
  // create() {
  //   const name = window.prompt("请输入标签名");
  //   if (!name) {
  //     return window.alert("标签名不能为空");
  //   }
  //   this.$store.commit("createTag", name);
  // }
}
</script>

<style lang="scss" scoped>
.tags {
  background: white;
  font-size: 14px;
  padding: 16px;
  display: flex;
  flex-grow: 1;
  flex-direction: column-reverse;
  > .current {
    display: flex;
    flex-wrap: wrap;

    > li {
      $bg: #d9d9d9;
      background: $bg;
      $h: 24px;
      height: $h;
      line-height: $h;
      border-radius: $h/2;
      padding: 0 16px;
      margin-right: 12px;
      margin-top: 4px;
      &.selected {
        background: darken($bg, 50%);
        color: white;
      }
    }
  }
  > .new {
    padding-top: 16px;
    button {
      //背景透明
      background: transparent;
      border: none;
      border-bottom: 1px solid;
      color: #999;
      padding: 0 3px;
    }
  }
}
</style>