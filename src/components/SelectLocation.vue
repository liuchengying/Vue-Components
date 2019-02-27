<template>
  <div class="content">
    <div class="content-top-div">
      <my-input :placeholder="placeholder"></my-input>
      <span class="input-right-text">取消</span>
    </div>
    <div>
      <my-menu
        :menuData="menuItem"
        @item-click="itemClick"
        position="right-bottom"
      ></my-menu>
    </div>

    <city-scroll></city-scroll>
  </div>
</template>

<script>
import myMenu from "./menu/myMenu";
import cityScroll from "./cityscroll/cityScroll";
import myInput from "./input/myInput"
export default {
  name: "SelectLocation",
  components: {
    myMenu: myMenu,
    cityScroll: cityScroll,
    myInput: myInput
  },
  data() {
    return {
      placeholder: '请输入城市名称',
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
    itemClick: function (item, index) {
      console.log(item.name);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.content-top-div {
  position: fixed;
  z-index: 9999;
  top: 0;
  width: 100%;
  height: 100px;
  background: #ecf0f1;
  padding-top: 50px;
  box-sizing: border-box;
  text-align: left;
  padding-left: 15px;
  padding-right: 15px;
  white-space: nowrap;
  overflow: hidden;
}

.input-right-text {
  display: inline-block;
  margin-left: 10px;
}
</style>
