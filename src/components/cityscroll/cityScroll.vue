<template>
  <div class="scroll-div" @touchend="end" @touchmove="move">
    <div class="for-class" v-for="(item, index) in this.baseDataArray" :key="index">
      <div
        class="info-line"
        :id="item.idName"
        :class="{'fixed-line': fixedItem === item.fixedItem}"
      >{{item.name}}</div>
      <div style="width: 100%;height: 45px" v-for="(o, index) in item.list" :key="index">{{o}}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dataArray: [],
      baseArray: [],
      baseDataArray: [],
      fixedItem: 0,
      scrollY: 0,
      timer: null,
      fromTop: 0
    };
  },
  mounted() {
    for (let i = 0; i < 98; i++) {
      this.dataArray[i] = i + 1;
    }
    this.baseArray = [...this.chunkArray(this.dataArray, 5, "range")];
    this.baseDataArray = [...this.chunkArray(this.dataArray, 5, "string")];
    this.baseDataArrayStr = [...this.chunkArray(this.dataArray, 5, "array")];
    for (let i = 0; i < this.baseArray.length; i++) {
      this.baseDataArray[i] = {
        name: this.baseArray[i],
        idName: `${this.baseDataArray[i]}-line`,
        fixedItem: i,
        list: this.baseDataArrayStr[i]
      };
    }
    this.$nextTick(() => {
      for (let item of this.baseDataArray) {
        item.top = document.getElementById(item.idName).offsetTop;
      }
      this.startScroll();
    });
    if(!('ontouchmove' in window)) {
        window.addEventListener("scroll", this.startScroll);
    }
    
  },
  methods: {
    move() {
      this.startScroll();
    },
    end() {
      this.TimeDelay();
    },
    TimeDelay() {
      if (this.timer) clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.scrollY =
          document.documentElement.scrollTop ||
          window.pageYOffset ||
          document.body.scrollTop;
        if (this.scrollY === this.fromTop) {
          clearTimeout(this.timer);
          return;
        }
        this.fromTop = this.scrollY;
        this.msg = this.scrollY;
        for (let item of this.baseDataArray) {
          if (this.scrollY > item.top - 20) {
            this.fixedItem = item.fixedItem;
          }
        }
        this.TimeDelay();
      }, 100);
    },
    chunkArray(arr, num, val) {
      if (!(arr instanceof Array) || !num) {
        return;
      }
      let arrayChunk = [];
      for (let i = 0; i <= arr.length / num; i++) {
        let arrItem = arr.slice(i * num, (i + 1) * num);
        if (val === "range") {
          arrayChunk.push(`${arrItem[0]}~${arrItem[arrItem.length - 1]}`);
        } else if (val === "string") {
          arrayChunk.push(arrItem.join(""));
        } else {
          arrayChunk.push(arrItem);
        }
      }
      return arrayChunk;
    },
    startScroll: function() {
      this.scrollY =
        document.documentElement.scrollTop ||
        window.pageYOffset ||
        document.body.scrollTop;
      for (let item of this.baseDataArray) {
        if (this.scrollY > item.top - 20) {
          this.fixedItem = item.fixedItem;
        }
      }
    }
  },
  destroyed() {
    if(!('ontouchmove' in window)) {
        window.removeEventListener("scroll", this.startScroll);
    }
  }
};
</script>

<style scoped>
.scroll-div {
  /* width: 100%; */
  overflow: auto;
  padding-top: 50px;
  padding-bottom: 180px;
}
.for-class {
  width: 100%;
}
.info-line {
  text-align: left;
  /* width: 100%; */
  height: 50px;
  line-height: 50px;
  padding-left: 15px;
  background-color: ghostwhite;
}
.fixed-line {
  position: fixed;
  width: 100%;
  top: 0px;
  z-index: 500;
}
</style>
