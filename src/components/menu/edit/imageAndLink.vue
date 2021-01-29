<template lang="">
<div class="edit-del-group">
  <div class="menu-btn" :disabled="disabled1" @click="dialogFormVisible = true" title="图片">
    <i class="edit-image-icon"></i>
    <span>图片</span></span>
  </div>
  <div class="menu-btn" :disabled="disabled2" @click="dialogTableVisible = true" title="链接">
    <i class="edit-link-icon"></i>
    <span>链接</span>
  </div>


  <el-dialog title="收货地址" :visible.sync="dialogFormVisible">

     <el-tabs v-model="activeName" @tab-click="handleClick">
    <el-tab-pane label="用户管理" name="first">用户管理</el-tab-pane>
    <el-tab-pane label="配置管理" name="second">
      <el-form :model="form">
    <el-form-item label="活动名称" :label-width="formLabelWidth">
      <el-input v-model="form.name" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="活动区域" :label-width="formLabelWidth">
      <el-select v-model="form.region" placeholder="请选择活动区域">
        <el-option label="区域一" value="shanghai"></el-option>
        <el-option label="区域二" value="beijing"></el-option>
      </el-select>
    </el-form-item>
  </el-form>
    </el-tab-pane>
  </el-tabs>
  
  <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
  </div>
</el-dialog>

</div>
</template>

<script>
import {
  mapGetters
} from 'vuex';
export default {
  name: 'imageAndLink',
   data() {
      return {
        dialogTableVisible: false,
        dialogFormVisible: false,
        form: {
          name: '',
          region: '',
          date1: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
        },
        formLabelWidth: '120px',
         activeName: 'second'
      };
    },
  computed: {
    ...mapGetters({
      'minder': 'getMinder',
      'editor': 'getEditor'
    }),
    disabled1() {
      return this.minder.queryCommandState && this.minder.queryCommandState('text') === -1;
    },
    disabled2() {
      return this.minder.queryCommandState && this.minder.queryCommandState('RemoveNode') === -1
    }
  },
  methods: {
    edit() {
      this.minder.queryCommandState('text') === -1 || this.editNode();
    },
    del() {
      this.minder.queryCommandState('RemoveNode') === -1 || this.minder.execCommand('RemoveNode');
    },
    editNode() {
      var editor = this.editor;
      var receiverElement = editor.receiver.element;
      var fsm = editor.fsm;
      var receiver = editor.receiver;

      receiverElement.innerText = this.minder.queryCommandValue('text');
      fsm.jump('input', 'input-request');
      receiver.selectAll();
    },
    handleClick(tab, event) {
        console.log(tab, event);
      }
  }
}
</script>
