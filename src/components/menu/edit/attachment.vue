<template lang="">
<div class="attachment-group">
  <div class="link">
    <el-button class="tab-icons insert attachment-group-el-button" :disabled="commandDisabled" @click="linkFormVisible = true" ></el-button>
    <el-dropdown trigger="click">
      <span class="el-dropdown-link" :disabled="commandDisabled">
        链接
        <i class="el-icon-caret-bottom el-icon--right"></i>
      </span>
      <el-dropdown-menu slot="dropdown" class="link-dropdown-list">
        <el-dropdown-item>插入链接</el-dropdown-item>
        <el-dropdown-item>移除已有链接</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </div>
  <div class="img">
    <el-button class="tab-icons insert attachment-group-el-button" :disabled="commandDisabled" @click="imageFormVisible = true" ></el-button>
    <el-dropdown trigger="click">
      <span class="el-dropdown-link" :disabled="commandDisabled">
        图片
        <i class="el-icon-caret-bottom el-icon--right"></i>
      </span>
      <el-dropdown-menu slot="dropdown" class="img-dropdown-list">
        <el-dropdown-item @click="imageFormVisible = true">插入图片</el-dropdown-item>
        <el-dropdown-item>移除已有图片</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </div>
  <div class="remark">
    <el-button class="tab-icons insert attachment-group-el-button " :disabled="commandDisabled"  @click="noteVisible = true"> </el-button>
    <el-dropdown trigger="click">
      <span class="el-dropdown-link" :disabled="commandDisabled">
        批注
        <i class="el-icon-caret-bottom el-icon--right"></i>
      </span>
      <el-dropdown-menu slot="dropdown" class="remark-dropdown-list">
        <el-dropdown-item>插入批注</el-dropdown-item>
        <el-dropdown-item>移除已有批注</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </div>

  <el-dialog title="图片" :visible.sync="imageFormVisible">
    <el-tabs class="attachment-group-tabs" v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="外链图片" name="first">
          <el-form :model="form">
            <el-row>
              <el-col :span="20">   <el-form-item label="图片链接:" :label-width="formLabelWidth">
                <el-input v-model="form.imageUrl" autocomplete="off" placeholder="必填：以 http(s):// 开头" @change="changeImageUrl"></el-input></el-input>
              </el-form-item>
              </el-col>
            </el-row>
          <el-row>
              <el-col :span="20">
                <el-form-item label="提示文案:" :label-width="formLabelWidth">
                  <el-input v-model="form.imageTitle" autocomplete="off" @change="changeImageTitle" placeholder="选填：鼠标在图片上悬停时提示的文本"></el-input>
                </el-form-item>
                </el-col>
          </el-row>
          <el-row>
              <el-col :span="20">
                <el-form-item label="图片预览:" :label-width="formLabelWidth">
                  <img  style="width:100px;height:100px" :src="this.$store.state.imageObj.imageUrl"   :error="errorImg" :key="this.$store.state.imageObj.imageTitle"  :preview-text="this.$store.state.imageObj.imageTitle">
                </el-form-item>
                </el-col>
          </el-row>
        </el-form>
      </el-tab-pane>
      <el-tab-pane label="上传图片" name="second">
        <el-form :model="form">
            <el-row :gutter="2">
              <el-col  > 
                <el-form-item  label="选择图片:" :label-width="formLabelWidth">
                  <el-button type="primary">上传<i class="el-icon-upload el-icon--right"></i></el-button>
                </el-form-item>
              </el-col>
            </el-row>
           <el-row :gutter="2" >
              <el-col :span="20">
                <el-form-item label="提示文案:" :label-width="formLabelWidth">
                  <el-input v-model="form.name" autocomplete="off"></el-input>
                </el-form-item>
                </el-col>
            </el-row>
             <el-row>
              <el-col :span="20">
                <el-form-item label="图片预览:" :label-width="formLabelWidth">
                  <img  style="width:100px;height:100px" :src="image.url" :key="image.title" :preview="image.preview" :preview-text="image.title">
                </el-form-item>
                </el-col>
          </el-row>
        </el-form>
      </el-tab-pane>
    </el-tabs>
        <div slot="footer">
          <el-button @click="imageFormVisible = false">取 消</el-button>
          <el-button type="primary" @click="insertImage">确 定</el-button>
        </div>
  </el-dialog>

  <el-dialog title="链接" :visible.sync="linkFormVisible">
          <el-form :model="form">
            <el-row>
              <el-col :span="20">   
                <el-form-item label="链接地址:" :label-width="formLabelWidth">
                <el-input v-model="form.linkUrl" autocomplete="off" placeholder="必填：以 http(s):// 开头" @change="changeImageUrl"></el-input></el-input>
              </el-form-item>
              </el-col>
            </el-row>
          <el-row>
              <el-col :span="20">
                <el-form-item label="提示文案:" :label-width="formLabelWidth">
                  <el-input v-model="form.linkTitle" autocomplete="off" @change="changeImageTitle" placeholder="选填：鼠标在图片上悬停时提示的文本"></el-input>
                </el-form-item>
                </el-col>
          </el-row>
        </el-form>
        <div slot="footer">
          <el-button @click="linkFormVisible = false">取 消</el-button>
          <el-button type="primary" @click="insertLink">确 定</el-button>
        </div>
  </el-dialog>

  <el-drawer
  title="批注"
  size="20%"
  :visible.sync="noteVisible"
  :direction="direction"
  :before-close="handleClose"
  class="note-drawer">
   <textarea   class="note-drawer-text" ref="note" style="width:100%;height:100%"  placeholder="备注">{{note}}</textarea>
</el-drawer>

</div>
</template>

<script>
import { mapGetters, mapActions, mapMutations } from 'vuex';
export default {
	name: 'attachment',
	data() {
		return {
			image: {
				url: this.$store.state.imageObj.imageUrl,
				title: this.$store.state.imageObj.imageTitle,
			},
			errorImg:
				'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg',
			linkFormVisible: false,
			imageFormVisible: false,
			noteVisible: false,
			direction: 'rtl',
      note: '',
			form: {
				name: '',
				imageUrl: '',
				imageTitle: '',
				linkUrl: '',
				linkTitle: '',
				region: '',
				date1: '',
				date2: '',
				delivery: false,
				type: [],
				resource: '',
				desc: '',
			},
			formLabelWidth: '120px',
			activeName: 'first',
		};
	},
	computed: {
		...mapGetters({
			minder: 'getMinder',
			count: 'count',
		}),
		commandDisabled() {
			var minder = this.minder;
			try {
				this.note = minder.queryCommandValue('note');
				if (this.note != '' && this.note != undefined) {
					this.noteVisible = true;
				}
			}catch(e){
        console.log(e)
      }
			minder.on &&
				minder.on('interactchange', function () {
					this.commandValue = minder.queryCommandValue('priority');
					// this.note = minder.queryCommandValue('note');
					// if (this.note != '' && this.note != undefined) {
					//   console.log(this.note)
					// 	this.noteVisible = true;
					// }
				});
			return (
				minder.queryCommandState &&
				minder.queryCommandState('priority') === -1
			);
		},
	},
	methods: {
		...mapActions(['changeCount', 'increment', 'clearMemory', 'setMemory']),
		test(key, value) {
			this.clearMemory(key, value);
		},
		showOverlay() {
			this.$msgbox({
				title: '输入',
				message: '暂时未实现，敬请期待！',
				showCancelButton: true,
				confirmButtonText: '确定',
				cancelButtonText: '取消',
				beforeClose: (action, instance, done) => {
					if (action === 'confirm') {
						instance.confirmButtonLoading = true;
						instance.confirmButtonText = '执行中...';
						setTimeout(() => {
							done();
							setTimeout(() => {
								instance.confirmButtonLoading = false;
							}, 300);
						}, 3000);
					} else {
						done();
					}
				},
			}).then((action) => {});
		},
		handleClick(tab, event) {
			console.log(tab, event);
		},
		changeImageUrl(e) {
			this.$store.commit('setImageUrl', e);
		},
		changeImageTitle(e) {
			this.$store.commit('setImageTitle', e);
		},
		insertImage() {
			this.imageFormVisible = false;
			console.log(this.$store.state.imageObj.imageTitle);
			this.minder.execCommand(
				'image',
				this.$store.state.imageObj.imageUrl,
				this.$store.state.imageObj.imageTitle
			);
		},

		insertLink() {
			this.linkFormVisible = false;
			this.minder.execCommand(
				'hyperlink',
				this.form.linkUrl,
				this.form.linkTitle
			);
			this.form.linkUrl = '';
			this.form.linkTitle = '';
		},
		handleClose(done) {
			if (this.$refs.note.value != '' && this.$refs.note.value != undefined) {
				this.minder.execCommand('note', this.$refs.note.value);
			}
			done();
			// 	this.$confirm('确认关闭？')
			// 		.then((_) => {
			// 			done();
			// 		})
			// 		.catch((_) => {});
		},
	},
};
</script>
