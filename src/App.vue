<template>
  <div id="app">
    <a-layout id="components-layout-demo-top-side-2">
      <a-layout>
        <a-layout-sider :collapsed='false' :width="sidebarWidth">
          <a-menu
            mode="inline"
            :default-selected-keys="selectedMenuIds"
            :default-open-keys="expandMenuIds"
            :style="{ height: '100vh', borderRight: 0, overflowY: 'scroll', overflowX: 'hidden', paddingLeft: '15px' }"
          >
            <template v-for="item in menus">
              <a-menu-item v-if="!item.submenus || item.submenus.length < 1" :key="item.id" @click="menuItemClick(item)">
                {{item.name}}
              </a-menu-item>
              <sub-menu v-else :menu-info="item" :key="item.id" :menu-click-handler="menuItemClick"></sub-menu>
            </template>
          </a-menu>
        </a-layout-sider>
        <a-layout style="overflow: hidden;">
          <a-layout-content style="overflow: hidden; background: #fff;">
            <div class="markdown-body">
              <VueMarkdown :source="markdownData"></VueMarkdown>
            </div>
          </a-layout-content>
        </a-layout>
      </a-layout>
    </a-layout>
  </div>
</template>

<script>
import axios from 'axios'
import VueMarkdown from 'vue-markdown'
import 'github-markdown-css/github-markdown.css'
import { Menu } from "ant-design-vue";

const SubMenu = {
  template: `
      <a-sub-menu :key="menuInfo.id" v-bind="$props" v-on="$listeners">
        <span slot="title">
          <span>{{ menuInfo.name }}</span>
        </span>
        <template v-for="item in menuInfo.submenus">
          <a-menu-item v-if="!item.submenus || item.submenus.length < 1" :key="item.id" @click="menuClickHandler(item)">
            <span>{{ item.name }}</span>
          </a-menu-item>
          <sub-menu v-else :key="item.id" :menu-info="item" />
        </template>
      </a-sub-menu>
    `,
  name: "SubMenu",
  // must add isSubMenu: true
  isSubMenu: true,
  props: {
    ...Menu.SubMenu.props,
    // Cannot overlap with properties within Menu.SubMenu.props
    menuInfo: {
      type: Object,
      default: () => ({})
    },
    menuClickHandler: {}
  }
};

export default {
  name: 'app',
  components: {
    VueMarkdown,
    "sub-menu": SubMenu
  },
  data() {
    return {
      collapsed: false,
      sidebarWidth: 230,
      markdownData: null,
      firstMenuItem: null,
      selectedMenuIds: [],
      expandMenuLevel: 1,
      expandMenuIds: [],
      menus: []
    };
  },
  mounted() {
    this.loadConfig(() => {
      this.genExpandMenuIds(this.menus, 1);

      this.selectedMenuIds.length = 0;
      if (this.firstMenuItem != null) {
        this.selectedMenuIds.push(this.firstMenuItem.id);
        this.menuItemClick(this.firstMenuItem);
      }
    });
  },
  methods: {
    loadConfig(callback) {
      axios.get("/static/config.json").then(res => {
        document.title = res.data.title;
        this.sidebarWidth = res.data.topicWidth;
        this.expandMenuLevel = res.data.openLevel;
        this.menus = res.data.topics;
        if (typeof(callback) == "function") {
          callback();
        }
      });
    },
    genExpandMenuIds(menus, level) {
      if (level <= this.expandMenuLevel) {
        let index = 0;
        menus.forEach((item) => {
          if (item.submenus && item.submenus.length > 0) {
            this.expandMenuIds.push(item.id);
            this.genExpandMenuIds(item.submenus, level + 1);
          } else {
            if (index == 0 && this.firstMenuItem == null) {
              this.firstMenuItem = item;
            }
          }
          index++;
        });
      } else {
        let index = 0;
        menus.forEach((item) => {
          if (!(item.submenus && item.submenus.length > 0)) {
            if (index == 0 && this.firstMenuItem == null) {
              this.firstMenuItem = item;
            }
          }
          index++;
        });
      }
    },
    openMarkdown(mdfile) {
      axios.get(mdfile).then(res => {
        this.markdownData = res.data;
      });
    },
    menuItemClick(item) {
      if (item.src) {
        this.openMarkdown(item.src);
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Helvetica Neue', 'Helvetica', tahoma, 'Hiragino Sans GB', 'PingFang SC', 'STHeitiSC-Light', 'Microsoft YaHei', Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 0px;
  padding: 0px;
  height: 100vh;
  overflow-y: hidden;
}

#components-layout-demo-top-side-2 {
  height: 100%;
}

.ant-menu-inline .ant-menu-submenu-title {
  font-size: 14px;
  font-weight: bold;
}

.ant-menu-submenu-inline > .ant-menu-submenu-title .ant-menu-submenu-arrow {
  margin-left: 10px;
  right: inherit !important;
}

.ant-menu-submenu-selected {
  color: rgba(0, 0, 0, 0.65) !important;
}

.ant-menu .ant-menu-item-selected {
  background: transparent !important;
}

.ant-menu-inline .ant-menu-item-selected::after {
  opacity: 0 !important;
}

.markdown-body {
  height: 100%;
  overflow-y: scroll;
  box-sizing: border-box;
  min-width: 200px;
  margin: 0 auto;
  padding: 0 45px;
}

@media (max-width: 767px) {
  .markdown-body {
    padding: 15px;
  }
}
</style>
