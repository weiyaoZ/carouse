<template>
  <div class="container" @mouseenter="clearTimer" @mouseleave="startTimer">
    <div class="imgList" ref="imgList">
      <img src="./img1/5.jpg" />
      <img src="./img1/1.jpg" />
      <img src="./img1/2.jpg" />
      <img src="./img1/3.jpg" />
      <img src="./img1/4.jpg" />
      <img src="./img1/5.jpg" />
      <img src="./img1/1.jpg" />
    </div>

    <div class="point" ref="point">
      <a href="javascript:;" class="active"></a>
      <a href="javascript:;"></a>
      <a href="javascript:;"></a>
      <a href="javascript:;"></a>
      <a href="javascript:;"></a>
    </div>

    <a href="javascript:;" class="arrow left" @click="prev">&lt;</a>
    <a href="javascript:;" class="arrow right" @click="next">&gt;</a>
  </div>
</template>

<script>
export default {
  name: "Child",
  data() {
    return {
      PAGE_WIDTH: 500,
      TIME: 400,
      ITEM_TIME: 20,
      moving: false,
      timer: null,
      index: 0,
    };
  },
  mounted() {
    this.timer = setInterval(() => {
      this.carousel(true);
    }, 3000);
  },
  methods: {
    startTimer() {
      this.timer = setInterval(() => {
        this.carousel(true);
      }, 3000);
    },
    clearTimer() {
      clearInterval(this.timer);
    },
    prev() {
      this.carousel(false);
    },

    next() {
      this.carousel(true);
    },

    // 轮播图函数
    carousel(event) {
      if (this.moving) {
        return;
      }
      this.moving = true;
      let offset = 0;
      if (typeof event === "boolean") {
        offset = event ? -this.PAGE_WIDTH : this.PAGE_WIDTH;
      } else {
        offset = -(event - this.indx);
      }
      let item_offset = offset / (this.TIME / this.ITEM_TIME);
      let current = this.$refs.imgList.offsetLeft;
      let target = current + offset;
      const timeer = setInterval(() => {
        current += item_offset;
        if (current == target) {
          clearInterval(timeer);
          this.moving = false;

          if (current == -(this.imgCount + 1) * this.PAGE_WIDTH) {
            current = -this.PAGE_WIDTH;
          } else if (current == 0) {
            current = -this.imgCount * this.PAGE_WIDTH;
          }
        }
        this.$refs.imgList.style.left = current + "px";
      }, this.ITEM_TIME);
    },
  },

  computed: {
    imgCount() {
      return document.querySelectorAll(".point>a").length;
    },
  },
};
</script>

<style scoped>
a {
  text-decoration: none;
}

.container {
  width: 500px;
  height: 332px;
  margin: 30px auto;
  position: relative;
  /* overflow: hidden; */
}

.container:hover .arrow {
  display: block;
}

.imgList {
  display: flex;
  position: absolute;
  left: -500px;
}

.point {
  width: 150px;
  height: 10px;
  display: flex;
  z-index: 3;
  position: absolute;
  left: 170px;
  bottom: 10px;
  justify-content: space-around;
}

.point a {
  width: 25px;
  height: 10px;
  border-radius: 20px;
  background-color: rgb(141, 133, 133);
}

.point .active {
  background-color: rgb(77, 30, 30, 0.6);
}

.arrow {
  width: 45px;
  height: 45px;
  line-height: 45px;
  border-radius: 50%;
  position: absolute;
  top: 150px;
  text-align: center;
  color: #fff;
  font-size: 27px;
  background-color: RGBA(0, 0, 0, 0.2);
  display: none;
}

.arrow:hover {
  background-color: RGBA(0, 0, 0, 0.5);
}

.left {
  left: 20px;
}

.right {
  right: 20px;
}
</style>

