<template>
    <div
    class="scroll-div"
    ref="scrollDiv"
      @touchend="end"
      @touchmove="move"
    >
      <div
        class="for-class"
        v-for="(item, index) in this.baseDataArray"
        :key="index"
        :id="item.idName + '__for-class'"
      >
        <div
          class="info-line"
          :id="item.idName"
          :class="{'fixed-line': fixedItem === item.fixedItem}"
        >{{item.name}}</div>
        <div
          style="width: 100%;height: 45px"
          v-for="(o, index) in item.list"
          :key="index"
        >{{o}}</div>
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
      fromTop: 0,
      scrollDivOffsetTop: 0,
      winWidth: 0,
      winHeight: 0,
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
      this.changeDomStyle();
      for (let item of this.baseDataArray) {
        item.top = document.getElementById(item.idName).offsetTop
      }
      this.startScroll();
    });
    if (!('ontouchmove' in window)) {
      window.addEventListener("scroll", this.startScroll, true)
    }

  },
  methods: {
    changeDomStyle: function () {
      this.scrollDivOffsetTop = this.$refs.scrollDiv.offsetTop
      // this.winWidth = window.innerWidth;
      // this.winHeight = window.innerHeight;
      // this.$refs.scrollDiv.style.height = this.winHeight - this.scrollDivOffsetTop + 'px';
      // this.$refs.scrollDiv.style.maxHeight = this.winHeight - this.scrollDivOffsetTop + 'px';
    },
    move: function () {
      this.startScroll();
    },
    end: function () {
      this.TimeDelay();
    },
    TimeDelay: function () {
      if (this.timer) clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.scrollY =
          document.documentElement.scrollTop ||
          window.pageYOffset ||
          document.body.scrollTop;
        // this.scrollY = this.$refs.scrollDiv.scrollTop;
        console.log(this.scrollY)
        if (this.scrollY === this.fromTop) {
          clearTimeout(this.timer);
          return;
        }
        this.fromTop = this.scrollY;
        this.msg = this.scrollY;
        for (let item of this.baseDataArray) {
          if (this.scrollY > item.top - 120) {
            this.fixedItem = item.fixedItem;
            document.getElementById(`${item.idName}`).style.top = this.scrollDivOffsetTop - 50 + 'px';
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
    startScroll: function () {
      this.scrollY =
        document.documentElement.scrollTop ||
        window.pageYOffset ||
        document.body.scrollTop;
      // this.scrollY = this.$refs.scrollDiv.scrollTop;
      console.log(this.scrollY)
      for (let item of this.baseDataArray) {
        if (this.scrollY > item.top - 120) {
          this.fixedItem = item.fixedItem;
          document.getElementById(`${item.idName}`).style.top = this.scrollDivOffsetTop - 50 + 'px';
        }
      }
    }
  },
  destroyed() {
    if (!('ontouchmove' in window)) {
      window.removeEventListener("scroll", this.startScroll, true);
    }
  }
};
</script>

<style scoped>
.scroll-div {
  margin-top: 150px;
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
  z-index: 999;
}
</style>
