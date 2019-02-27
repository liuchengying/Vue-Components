<template>
  <div class="main">
    <div class="menu-content" ref="mainMenu" @click="clickMenu">
      <img :src="menuImg" alt="menu" ref="MenuImgBg">
    </div>
    <div class="div-items" ref="mainItem">
      <div
        class="other-content"
        ref="menuItem"
        v-for="(item,index) in menuData"
        :key="index"
        :id="item.name"
        @click="clickItem(item, index)"
      >
        <img :src="item.src" :alt="index">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "myMeun",
  props: {
    menuImg: {
      default: require("../../assets/menu.png")
    },
    position: {
      type: String,
      default: "right-bottom"
    },
    width: {
      type: Number,
      default: 50
    },
    height: {
      type: Number,
      default: 50
    },
    diameter: {
      type: Number,
      default: 150
    },
    menuBg: {
      type: String,
      default: "white"
    },
    itemBg: {
      type: String,
      default: "white"
    },
    menuData: {
      type: Array
    }
  },
  data() {
    return {
      showImg: false,
      isUnfold: false,
      transformValue: ["+", "+"]
    };
  },
  mounted() {
    this.$refs.mainMenu.style.width = this.width + "px";
    this.$refs.mainMenu.style.height = this.height + "px";
    this.$refs.mainMenu.style.lineHeight = this.height + "px";
    this.$refs.mainMenu.style.background = this.menuBg;
    this.$refs.MenuImgBg.style.width = this.width - 12 + "px";
    this.$refs.MenuImgBg.style.width = this.height - 12 + "px";

    this.menuData.forEach((item, index) => {
      let el = document.getElementById(item.name);
      el.style.width = this.width * 0.8 + "px";
      el.style.height = this.height * 0.8 + "px";
      el.style.lineHeight = this.height * 0.8 + "px";
      el.style.background = this.itemBg;
    });

    switch (this.position) {
      case "right-top":
        this.$refs.mainMenu.style.right = "20px";
        this.$refs.mainMenu.style.top = "20px";
        this.$refs.mainItem.style.right = "20px";
        this.$refs.mainItem.style.top = "20px";
        this.transformValue = ["-", "+"];
        break;
      case "right-bottom":
        this.$refs.mainMenu.style.right = "20px";
        this.$refs.mainMenu.style.bottom = "20px";
        this.$refs.mainItem.style.right = "20px";
        this.$refs.mainItem.style.bottom = "20px";
        this.transformValue = ["-", "-"];
        break;
      case "left-top":
        this.$refs.mainMenu.style.left = "20px";
        this.$refs.mainMenu.style.top = "20px";
        this.$refs.mainItem.style.left = "20px";
        this.$refs.mainItem.style.top = "20px";
        this.transformValue = ["+", "+"];
        break;
      case "left-bottom":
        this.$refs.mainMenu.style.left = "20px";
        this.$refs.mainMenu.style.bottom = "20px";
        this.$refs.mainItem.style.left = "20px";
        this.$refs.mainItem.style.bottom = "20px";
        this.transformValue = ["+", "-"];
        break;
      default:
        break;
    }
  },
  methods: {
    clickMenu: function() {
      if (!this.isUnfold) {
        // this.showImg = true;
        this.menuData.forEach((item, index) => {
          this.startTransition(item.name, index, false);
        });
        this.$refs.mainMenu.style.transform = "rotate(360deg)";
      } else {
        this.menuData.forEach((item, index) => {
          this.startTransition(item.name, index, true);
        });
        this.$refs.mainMenu.style.transform = "rotate(0deg)";
        // this.showImg = false;
      }
      this.isUnfold = !this.isUnfold;
    },
    startTransition: function(name, index, revent) {
      let jiaodu = 90 / (this.menuData.length - 1);
      let axisX = Math.sin(
        ((this.menuData.length - 1 - index) * jiaodu * 2 * Math.PI) / 360
      );
      let axisY = Math.cos(
        ((this.menuData.length - 1 - index) * jiaodu * 2 * Math.PI) / 360
      );
      let el = document.getElementById(name);
      if (!revent) {
        setTimeout(() => {
          el.style.transitionDuration = "200ms";
          el.style.transform = `translate(${this.transformValue[0]}${axisX *
            this.diameter}px,${this.transformValue[1]}${axisY *
            this.diameter}px)`;
        }, index * 80);
      } else {
        el.style.transitionDuration = "200ms";
        el.style.transform = "translate(0,0)";
      }
    },
    clickItem: function(item, index) {
      this.$emit("item-click", item, index);
    }
  }
};
</script>

<style scoped>
.menu-content {
  position: fixed;
  z-index: 2000;
  border-radius: 50%;
  transition-duration: 400ms;
  text-align: center;
  border: solid grey 3px;
  box-shadow: aliceblue 1px 1px 1px;
  background-position: center center;
  vertical-align: middle;
  z-index: 10000;
  /* opacity: 0.7; */
}
.other-content {
  position: fixed;
  border-radius: 50%;
  z-index: 1000;
  border: solid grey 3px;
  text-align: center;
  box-shadow: aliceblue 1px 1px 1px;
  vertical-align: middle;
  opacity: 0.7;
}
img {
  vertical-align: middle;
}
img:active {
  transform: scale(1.1, 1.1);
}
.div-items {
  position: fixed;
  width: 55px;
  height: 55px;
  line-height: 55px;
  vertical-align: middle;
  text-align: center;
  border-radius: 50%;
  padding:3px;
  box-sizing: border-box;
  z-index: 9999;
}
</style>
