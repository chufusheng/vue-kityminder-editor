<template lang="">
	<div class="custom-box-group">
		<el-row>
			<el-col :span="24">
				<el-input placeholder="请输入内容"  :disabled="commandDisabled"  v-model="inputRes" class="input-with-select">
					<el-button @click="addResource" :disabled="commandDisabled" slot="append" icon="el-icon-circle-plus-outline"></el-button>
				</el-input>
			</el-col>
			<el-col :span="24">
				 <el-select
				  class="input-with-select"
				  :disabled="commandDisabled"
					v-model="thisRes"
					multiple
					placeholder="请选择"
					@visible-change="openRes"
					@change="removeOrAddTag"
					>
					<el-option
					v-for="item in options"
					:key="item.value"
					:label="item.label"
					:value="item.value">					
					<el-button class="input-with-select-button" :style="{padding:'4px 10px', 'font-size':'12px',background:item.color}">{{item.label}}</el-button> 
					</el-option>                

				</el-select>
	
			</el-col>
	  </el-row>  
	</div>
</template>

<script>
import { mapGetters, mapActions, mapMutations } from 'vuex';
export default {
	name: 'customBox',
	data() {
		return {
			inputRes: "",
			resourceOpthion: {},
			thisRes: [],
			used: [],
			useds: [],
			dialogTableVisible: false,
			isInteracting: false,
			options: []
		};
	},
	watch: {
		used() {
			var used = this.used
			if (minder.queryCommandState('resource') != -1) {
				var resource = used.filter(function (resource) {
					return resource.selected;
				}).map(function (resource) {
					return resource.name;
				});

				// 由于 interactchange 带来的改变则不用执行 resource 命令
				if (this.isInteracting) {
					return;
				}
				minder.execCommand('resource', resource);
			}
		}
	},
	computed: {
		...mapGetters({
			minder: 'getMinder',
			editor: 'getEditor'
		}),

		commandDisabled() {
			var minder = this.minder;
			try {
				this.thisRes = minder.queryCommandValue('resource');
			} catch (e) {

			}
			return (
				minder.queryCommandState &&
				minder.queryCommandState('priority') === -1
			);
		},


	},

	methods: {
		addResource() {

			var enabled = minder.queryCommandState('resource') != -1;
			var selected = enabled ? minder.queryCommandValue('resource') : [];
			this.used = minder.getUsedResource().map(function (resourceName) {
				return {
					name: resourceName,
					selected: selected.indexOf(resourceName) > -1
				}
			})
			var resourceName = this.inputRes
			var origin = minder.queryCommandValue('resource');


			if (!resourceName || !/\S/.test(resourceName)) return;

			if (origin.indexOf(resourceName) == -1) {
				this.used.push({
					name: resourceName,
					selected: true
				})
			}

		},
		openRes: function (callback) {
			if (callback) {
				var used = minder.getUsedResource()
				for (var i = 0; i < used.length; i++) {
					this.options.push({
						value: used[i],
						label: used[i],
						color: minder.getResourceColor(used[i]).toHEX()
					})
				}
				var enabled = minder.queryCommandState('resource') != -1;
				this.thisRes = enabled ? minder.queryCommandValue('resource') : [];
				this.options = unique(this.options)
				console.log(this.options)

			}


			function unique(array) {
				var obj = {};
				return array.filter(function (item, index, array) {
					return obj.hasOwnProperty(typeof item + JSON.stringify(item)) ? false : (obj[typeof item + JSON.stringify(item)] = true)
				})
			}

		},

		removeOrAddTag(tag) {
			minder.execCommand('resource', tag);
		},

		resourceColor(resource) {
			return minder.getResourceColor(resource).toHEX();
		},
	}
};
</script>
