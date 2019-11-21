<template>
  <div class="tabs" ref="content">
    <div class="tab-scroll">
      <ul>
        <li
          class="tab"
          v-for="(tab,i) in tabs"
          :key="tab.key"
          :style="{width:tabWidth +'px'}"
          :class="[{active:tab.key === value}]"
          @click="changeTab(tab)"
        >
          <span v-if="tab.favico" class="tab-favicon">
            <img :src="tab.favico" />
          </span>
          <span class="tab-content">{{tab.label}}</span>
          <div class="tab-close" @click.stop="TabDelete(tab,i)">
            <svg class="tab-close-icon" width="16" height="16" stroke="#595959">
              <path d="M 4 4 L 12 12 M 12 4 L 4 12" />
            </svg>
          </div>
        </li>
      </ul>
      <span class="add-new" @click="addTab">
        <svg class="add-icon" viewBox="0 0 1027 1024" width="20" height="20" stroke="#595959">
          <path
            d="M853.95951925 475.5066105h-310.08967642v-300.83326917c0-13.88461268-13.88461268-32.39742862-32.39742936-32.39742862-18.51281666 0-32.39742862 18.51281666-32.39742861 37.02563333v300.83326844H178.2417157c-18.51281666-4.62820399-37.02563333 13.88461268-37.02563262 32.39742934s18.51281666 32.39742862 32.39742863 32.39742861h300.83326916v300.83326845c4.62820399 18.51281666 18.51281666 37.02563333 37.02563261 37.02563334s32.39742862-18.51281666 32.39742935-32.39742862v-305.46147317h305.46147243c18.51281666 0 32.39742862-18.51281666 32.39742934-32.39742861s-9.25640796-37.02563333-27.76922535-37.02563333z"
          />
        </svg>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    tabs: {
      type: Array,
      default: () => []
    },
    minWidth: {
      type: Number,
      default: 40
    },
    maxWidth: {
      type: Number,
      default: 240
    },
    value: {
      type: [String, Number],
      default: ""
    }
  },
  data() {
    return {
      tabWidth: null
    };
  },
  mounted() {
    this.calcTabWidth();
    window.addEventListener("resize", this.handleResize);
  },
  destroyed() {
    window.removeEventListener("resize", this.handleResize);
  },
  methods: {
    calcTabWidth() {
      let { tabs, maxWidth, minWidth } = this;
      let $content = this.$refs.content;
      if (!$content) return Math.max(maxWidth, minWidth);
      let contentWidth = $content.clientWidth - 50;
      let width = contentWidth / tabs.length;
      if (width > maxWidth) width = maxWidth;
      if (width < minWidth) width = minWidth;
      this.tabWidth = width;
    },
    changeTab(tab) {
      this.$emit("input", tab.key);
    },
    TabDelete(tab, i) {
      let { tabs } = this;
      let index = tabs.findIndex(item => item.key === tab.key);
      let after, before;
      if (i === index) {
        after = tabs[i + 1];
        before = tabs[i - 1];
      }
      if (after) {
        this.$emit("input", after.key);
      } else if (before) {
        this.$emit("input", before.key);
      } else if (tabs.length <= 1) {
        this.$emit("input", null);
      }
      tabs.splice(i, 1);
      this.$nextTick(() => {
        this.calcTabWidth();
      });
    },
    addTab() {
      let newTab = {
        label: "New Tab",
        key: "tab" + Date.now()
      };
      this.tabs.push(newTab);
      this.$emit("input", this.tabs[this.tabs.length - 1].key);
      this.$nextTick(() => {
        this.calcTabWidth();
      });
    },
    handleResize() {
      this.calcTabWidth();
    }
  }
};
</script>

<style lang="scss" scoped>
.tabs {
  $bg: #dee1e6;
  $speed: 150ms;

  padding-top: 10px;
  height: 100%;
  background-color: #eeeeee;
  position: relative;
  border-bottom: 1px solid #e4e7ed;
  margin: 0;

  .tab-scroll {
    height: 100%;
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: center;
    margin-bottom: -1px;
  }

  ul {
    display: flex;
    height: 100%;
    position: relative;
    margin: 0;
    padding: 0;
    list-style: none;
    box-sizing: border-box;
    float: left;
    border: 1px solid #e4e7ed;
    border-bottom: none;
    overflow: hidden;

    .tab {
      display: flex;
      height: 34px;
      line-height: 34px;
      list-style: none;
      position: relative;
      align-items: center;
      overflow: hidden;
      box-sizing: border-box;
      cursor: pointer;

      &:hover {
        background-color: #f2f3f5;
        border-bottom: 1px solid #e4e7ed;

        .tab-close {
          width: 16px;
        }
      }
      &.active {
        background-color: #fff;
        border-bottom: none;

        .tab-close {
          width: 16px;
        }
      }
    }

    .tab:nth-of-type(n + 2) {
      border-left: 1px solid #e4e7ed;
    }
  }

  .tab-close {
    display: flex;
    align-items: center;
    height: 16px;
    width: 0;
    z-index: 1;
    margin-left: auto;
    margin-right: 10px;
    transition: all 0.3s;
  }
  .tab-close-icon {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    &:hover {
      stroke: #000;
      background-color: #e8eaed;
    }
  }

  .tab-content {
    flex-grow: 1;
    margin-left: 7px;
    text-align: left;
    user-select: none;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: clip;
    box-sizing: border-box;
  }
  .tab-favicon {
    height: 16px;
    margin-left: 21px;
    display: flex;
    align-items: center;
    overflow: hidden;
    box-sizing: border-box;

    img {
      height: 100%;
    }
  }
  .add-new {
    width: 28px;
    height: 28px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-left: 10px;
    margin-right: 10px;
    border-radius: 50%;
    &:hover {
      background-color: #dddddd;
      cursor: pointer;
      .add-icon {
        stroke: #000;
        // background-color: #0288e9;
      }
    }
  }
}
</style>