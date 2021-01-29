<template lang="">
<div class="theme-group">
  <!-- <el-row> -->
    <!-- <el-col :span="24">      
      <span class="demonstration">主题</span>
    </el-col> -->
     <!-- <el-col > -->
      <!-- <el-dropdown trigger="click" :hide-on-click="true" class="dropdown-toggle theme-icons menu-btn" @command="handleCommand">
        <span class="el-dropdown-link ">
        {{current_theme}}
      <i class="el-icon-caret-bottom el-icon--right"></i>
    </span>
        <el-dropdown-menu slot="dropdown" class="theme-dropdown-list">
            <el-dropdown-item v-for="(item,index) in themeList"
                              v-text="item"
                              :command="index"
            >{{item}}</el-dropdown-item>        
        </el-dropdown-menu>
      </el-dropdown>  -->
      <el-select @change="minder.execCommand('theme', rootData.theme)" v-model="rootData.theme" style="width:90px">
        <el-option v-for="(item, index) in themeList" :key="index" :value="index" :label="item">
          <el-button :style="getThemeThumbStyle(index)" type="" size="mini" round >{{ item }}</el-button>
        </el-option>
      </el-select>
    <!-- </el-col> -->

  <!-- </el-row> -->
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
        theme: "fresh-blue",
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

  methods: {
    handleCommand(command) {
      this.current_theme = command;
      console.log();
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