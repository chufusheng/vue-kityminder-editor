<template lang="">
<div class="theme-group">
      <el-select  @change="minder.execCommand('theme', rootData.theme)" v-model="rootData.theme" style="width:90px">
        <el-option v-for="(item, index) in themeList" :key="index" :value="index" :label="item">
          <el-button :style="getThemeThumbStyle(index)" type="" size="mini" round >{{ item }}</el-button>
        </el-option>
      </el-select>
</div>

</template>

<script>
import { mapGetters } from "vuex";
export default {
	name: "theme",
	data() {
		return {
			theme_index: 1,
			current_theme: "脑图经典",
			ulActive: false,
			themeList: {
				"classic": "脑图经典",
				"classic-compact": "紧凑经典",
				snow: "温柔冷光",
				"snow-compact": "紧凑冷光",
				fish: "鱼骨图",
				wire: "线框",
				"fresh-red": "清新红",
				"fresh-soil": "泥土黄",
				"fresh-green": "文艺绿",
				"fresh-blue": "天空蓝",
				"fresh-purple": "浪漫紫",
				"fresh-pink": "脑残粉",
				"fresh-red-compat": "紧凑红",
				"fresh-soil-compat": "紧凑黄",
				"fresh-green-compat": "紧凑绿",
				"fresh-blue-compat": "紧凑蓝",
				"fresh-purple-compat": "紧凑紫",
				"fresh-pink-compat": "紧凑粉",
				tianpan: "经典天盘",
				"tianpan-compact": "紧凑天盘",
			},
			rootData: {
				template: "structure",
				theme: "",
			},
		};
	},

	computed: {
		...mapGetters({
			minder: "getMinder",
		}),
		// class_theme_index: function () {
		//   return "theme-" + this.theme_index;
		// },
	},

	mounted: function () {
		this.createcode();//需要触发的函数
	},

	methods: {

		createcode() {
			// console.log(this.minder.queryCommandValue('Theme'))
		},

		handleCommand(command) {
			this.current_theme = command;
			this.minder.execCommand("theme", command);
		},
		getThemeThumbStyle(theme) {
			var themeList = kityminder.Minder.getThemeList();
			var themeObj = themeList[theme];
			if (!themeObj) {
				return;
			}
			var style = {
				color: themeObj["root-color"],
				"border-radius": themeObj["root-radius"] / 2,
				width: "70Px",
				"margin-bottom": "60px",
			};

			if (themeObj["root-background"]) {
				style["background"] = themeObj["root-background"].toString();
			}

			return style;
		},
	},
};
</script>