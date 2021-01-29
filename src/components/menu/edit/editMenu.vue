<template lang="html">
<div class="menu-container">
  <expand></expand>
  <selection></selection>
  <insert-box></insert-box>
  <move-box></move-box>
  <edit-del></edit-del>
  <imageAndLink></imageAndLink>
  <sequence-box></sequence-box>
  <progress-box></progress-box>
  <div class='menu-button'>  
    <el-button type="primary" @click="save" size="medium">保存</el-button>
  </div>
</div>
</template>

<script>
import insertBox from "./insertBox";
import moveBox from "./moveBox";
import editDel from "./editDel";
import imageAndLink from "./imageAndLink"
import sequenceBox from "./sequenceBox";
import progressBox from "./progressBox";
import expand from "./expand";
import selection from "./selection";

export default {
  name: "editMenu",
  components: {
    insertBox,
    moveBox,
    editDel,
    sequenceBox,
    progressBox,
    expand,
    selection,
    imageAndLink
  },
  methods: {
    save: function () {
      this.$parent.save();
    },
  },
};
</script>
