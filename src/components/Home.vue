<template>
  <div class="select-location-main">
    <span @click="routerJump(routerUrl.location)">地区选择组件</span>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      routerUrl: {
        location: '/location-select'
      },
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
    },
    routerJump: function (pathName) {
      console.log(pathName)
      this.$router.push({path: pathName})
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.select-location-main {
  height: 100%;
  padding: 50px 15px;
  text-align: center;
}
.select-location-main > span {
  display: block;
  height: 50px;
  line-height: 50px;
  font-size: 1.25rem;
  background: #2ecc71;
  color:#ecf0f1;
  box-shadow: #30336b 2px 2px 2px;
}

.select-location-main > span:active {
  background: #27ae60;
  box-shadow: #30336b 0.5px 0.5px 0.5px;
}
</style>
