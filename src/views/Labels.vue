<template>
  <Layout>
    <div class="tags">
      <router-link
        class="tag"
        v-for="tag in tags"
        :key="tag.id"
        :to="`/labels/edit/${tag.id}`"
      >
        <span>{{ tag.name }}</span>
        <Icon name="right" />
      </router-link>
    </div>
    <div class="createTag-wrapper">
      <!--  createTag从TagHelper里来  -->
      <Button class="createTag" @click="createTag">新建标签</Button>
    </div>
  </Layout>
</template>

<script lang="ts">
import Icon from "@/components/Icon.vue";
import Layout from "@/components/Layout.vue";
import Vue from "vue";
import Button from "@/components/Button.vue";
import { Component } from "vue-property-decorator";
import { mixins } from "vue-class-component";
import TagHelper from "@/mixins/TagHelper";

@Component({
  components: { Layout, Icon, Button },
})
export default class Labels extends mixins(TagHelper) {
  get tags() {
    return this.$store.state.tagList;
  }
  //提取获取一下数据
  beforeCreate() {
    this.$store.commit("fetchTags");
  }
  // createTag() {
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
  font-size: 16px;
  padding-left: 16px;
  padding-right: 16px;

  > .tag {
    min-height: 44px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid #e6e7e7;
  }
  svg {
    width: 18px;
    height: 18px;
    // margin-right: 16px;
    color: #666;
  }
}
.createTag {
  // background: #767676;
  color: white;
  padding: 0 16px;
  height: 40px;
  border: none;
  border-radius: 4px;
  &-wrapper {
    padding: 16px;
    margin-top: 44-16px;
    text-align: center;
  }
}
</style>