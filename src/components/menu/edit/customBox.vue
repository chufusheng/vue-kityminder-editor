<template lang="">
<div class="custom-box-group">
  <!-- <div class="image menu-btn" @click="resetlayout" :disabled="disabled"> -->
    <!-- <el-row>
      <el-col>
        <el-input placeholder="请输入内容"  class="input-with-select">  </el-input>
        <el-button  icon="el-icon-search"></el-button>
      </el-col>
      <el-col >        
        <el-input placeholder="请输入内容"  class="input-with-select">  </el-input>
      </el-col>
    </el-row>
        </div> -->
        <el-row>
          <el-col :span="24">
              <el-input placeholder="请输入内容" v-model="input3" class="input-with-select">
                <el-button slot="append" icon="el-icon-circle-plus-outline"></el-button>
              </el-input>
          </el-col>
          <el-col :span="24">
            <el-input   class="input-with-select">  </el-input>
          </el-col>
        </el-row>
     
</div>
</template>

<script>
import { mapGetters } from 'vuex';
export default {
	name: 'customBox',
	data() {
		return {
			dialogTableVisible: false,
		};
	},
	computed: {
		...mapGetters({
			minder: 'getMinder',
			editor: 'getEditor',
		}),

		disabled2() {
			return (
				this.minder.queryCommandState &&
				this.minder.queryCommandState('RemoveNode') === -1
			);
		},
	},
	methods: {
		edit() {
			this.minder.queryCommandState('text') === -1 || this.editNode();
		},
		del() {
			this.minder.queryCommandState('RemoveNode') === -1 ||
				this.minder.execCommand('RemoveNode');
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
		},
	},
};
</script>
