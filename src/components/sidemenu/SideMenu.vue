<template>
  <div class="sidebar">
    <!-- 左侧二级菜单栏的组件封装 -->
    <el-menu
      class="sidebar-el-menu"
      :default-active="toIndex()"
      background-color="white"
      text-color="#7a8297"
      active-text-color="#2d8cf0"
      router
    >
      <div
        v-for="item in items"
        :key="item.index"
      >
        <!-- 二级菜单 -->
        <el-menu-item
          :index="item.index"
          :key="item.index"
          v-if="item.nextItem == undefined"
        >
          <!-- 需要图标的在 item 对象中加上属性 icon -->
          <!-- <i :class="item.icon"></i> -->
          <span slot="title">{{ item.title }}</span>
        </el-menu-item>
        <!-- 三级菜单 -->
        <el-submenu
          :index="item.index"
          v-else
        >
          <span slot="title">{{item.title}}</span>
          <el-menu-item-group>
            <div
              v-for="itemsTwo in item.nextItem"
              :key="itemsTwo.index"
            >
              <el-menu-item
                :index="itemsTwo.index"
                :key="itemsTwo.index"
                v-if="itemsTwo.nextItems == undefined"
              >
                {{itemsTwo.title}}
              </el-menu-item>
              <!-- 四级菜单 -->
              <el-submenu
                :index="itemsTwo.index"
                v-else
              >
                <span slot="title">{{itemsTwo.title}}</span>
                <el-menu-item-group>
                  <el-menu-item
                    v-for="itemsThree in itemsTwo.nextItems"
                    :index="itemsThree.index"
                    :key="itemsThree.index"
                    class="three-title"
                  >
                    {{itemsThree.title}}
                  </el-menu-item>
                </el-menu-item-group>
              </el-submenu>
            </div>
          </el-menu-item-group>
        </el-submenu>

      </div>
    </el-menu>
  </div>
</template>

<script>
export default {
    props: ['items'],
    data() {
        return {};
    },
    methods: {
        // 根据路径绑定到对应的二级菜单，防止页面刷新重新跳回第一个
        toIndex() {
            return this.$route.path.split('/')[2];
        }
    }
};
</script>

<style scoped>
/* 左侧菜单栏定位和位置大小设定 */
.sidebar {
    display: block;
    position: absolute;
    left: 0;
    top: 70px;
    bottom: 0;
    overflow-y: scroll;
}
.sidebar::-webkit-scrollbar {
    width: 0;
}
.sidebar-el-menu {
    width: 250px;
}
.sidebar > ul {
    height: 100%;
}

/* 左侧二级菜单项的样式 */
.el-menu-item {
    font-size: 14px !important;
    padding-left: 35px !important;
    color: black !important;
}

/* 左侧二级菜单选中时的样式 */
.el-menu-item.is-active {
    color: white !important;
    background: #3989fa !important;
}
.el-menu-item,
.el-submenu__title {
    height: 50px !important;
    line-height: 50px !important;
}
.three-title {
    padding-left: 60px !important;
}
</style>