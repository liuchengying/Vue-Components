<template>
  <div class="hello">
    <my-menu :menuData="menuItem" @item-click="itemClick" position="right-bottom"></my-menu>
    <city-scroll></city-scroll>
  </div>
</template>

<script>
import myMenu from "./menu/myMenu";
import cityScroll from "./cityscroll/cityScroll";
export default {
  name: "HelloWorld",
  components: {
    myMenu: myMenu,
    cityScroll: cityScroll
  },
  data() {
    return {
      msg: "Welcome to Your Vue.js App",
      menuItem: [
        {
          name: "item1",
          src: require("../assets/icon_setting.png")
        },
        {
          name: "item2",
          src: require("../assets/icon_qq.png")
        },
        {
          name: "item3",
          src: require("../assets/icon_patriarch.png")
        },
        {
          name: "item4",
          src: require("../assets/icon_gift.png")
        },
        {
          name: "item5",
          src: require("../assets/icon_alipay_line.png")
        }
      ]
    };
  },
  methods: {
    itemClick: function(item, index) {
      console.log(item.name);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
