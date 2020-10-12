<template>
  <div id="app">
    <div class="swiper" @mouseenter="clearTime()" @mouseleave="autoPlay()">
      <div class="swiper-img" ref="swiperImg">
        <!-- <template v-for="(item, index) in imgs">
          <img :src="item.src" alt="" :key="index" v-show="currt == index" />
        </template> -->
        <!-- <img :src="imgs[currt].src" alt="" /> -->
      </div>
      <div class="swiper-menu">
        <div class="btn" @click="rightBtn()"></div>
        <div class="btn" @click="leftBtn()"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      imgs: [
        {
          src: require("./assets/timg.jpg"),
        },
        {
          src: require("./assets/timg2.jpg"),
        },
        {
          src: require("./assets/timg3.jpg"),
        },
      ],
      currt: 0,
      imgLen: 0,
      time: null,
      btnFlag: true,
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.fragment();
      this.autoPlay();
    },
    rightBtn() {
      this.currt++;
      this.currtFun();
    },
    leftBtn() {
      this.currt--;
      this.currtFun();
    },
    currtFun() {
      let random = this.randomFun(1, 3);
      if (this.currt >= this.imgs.length) {
        this.currt = 0;
      }

      if (this.currt < 0) {
        this.currt = this.imgs.length - 1;
      }
      if (random == 1) {
        this.clickFun();
      } else if (random == 2) {
        this.salcFun();
      } else {
        this.flipFun();
      }
    },
    fragment() {
      let swiperImg = this.$refs.swiperImg;
      let number = 6;
      let domStr = `<div class="swiperFram"></div>`;
      let swiperW = swiperImg.offsetWidth;
      let nowSrc = this.imgs[this.currt].src;
      let sWidth = `${parseInt(swiperW / number)}`;
      swiperImg.innerHTML = "";
      for (let i = 0; i < number; i++) {
        swiperImg.innerHTML += domStr;
        swiperImg.children[i].style.width = `${sWidth}px`;
        swiperImg.children[i].style.left = `${i * sWidth}px`;
        swiperImg.children[i].style.backgroundImage = `url(${nowSrc})`;
        swiperImg.children[i].style.backgroundRepeat = `no-repeat`;
        // swiperImg.children[i].style.backgroundSize = `cover`;
        swiperImg.children[i].style.backgroundPosition = `${-i * sWidth}px`;
      }
    },
    clickFun() {
      let _that = this;
      this.btnFlag = false;
      _that.$nextTick(() => {
        let swiperImg = _that.$refs.swiperImg;
        let swiperHeight = swiperImg.offsetHeight;
        let swiperChilder = swiperImg.children;
        let nowSrc = _that.imgs[_that.currt].src;
        for (let i = 0; i < swiperChilder.length; i++) {
          swiperImg.children[i].style.backgroundImage = `url(${nowSrc})`;
          swiperImg.children[i].style.top =
            i % 2 == 0 ? `${swiperHeight}px` : `-${swiperHeight}px`;
          swiperImg.children[i].addEventListener("transitionend", function () {
            swiperImg.children[i].style.top = 0;
          });
        }
      });
    },
    flipFun() {
      let _that = this;
      this.btnFlag = false;
      _that.$nextTick(() => {
        let swiperImg = _that.$refs.swiperImg;
        let swiperChilder = swiperImg.children;
        let nowSrc = _that.imgs[_that.currt].src;
        for (let i = 0; i < swiperChilder.length; i++) {
          swiperImg.children[i].style.backgroundImage = `url(${nowSrc})`;
          swiperImg.children[i].style.transform = `rotateY(90deg)`;
          swiperImg.children[i].addEventListener("transitionend", function () {
            swiperImg.children[i].style.transform = `rotateY(0)`;
          });
        }
      });
    },
    salcFun() {
      let _that = this;
      this.btnFlag = false;
      _that.$nextTick(() => {
        let swiperImg = _that.$refs.swiperImg;
        let swiperChilder = swiperImg.children;
        let nowSrc = _that.imgs[_that.currt].src;
        swiperImg.style.transform = `scale(1.1)`;
        for (let i = 0; i < swiperChilder.length; i++) {
          swiperImg.children[i].style.backgroundImage = `url(${nowSrc})`;
        }
        swiperImg.addEventListener("transitionend", function () {
          swiperImg.style.transform = `scale(1)`;
        });
      });
    },
    randomFun(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },
    autoPlay() {
      let _that = this;
      this.time = setInterval(() => {
        _that.currt++;
        _that.currtFun();
      }, 3000);
    },
    clearTime() {
      clearInterval(this.time);
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.swiper {
  width: 100%;
  height: 100vh;
  position: relative;
  overflow: hidden;
}
.swiper-img {
  width: 100%;
  height: 100%;
  transition: all 0.5s;
}
img {
  vertical-align: top;
  width: 100%;
  height: 100%;
}
.swiper-menu {
  position: absolute;
  top: 50%;
  left: 0;
  width: 100%;
  transform: translateY(-50%);
  display: flex;
  justify-content: space-between;
}
.btn {
  width: 50px;
  height: 50px;
  text-align: center;
  line-height: 50px;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 50%;
  color: #fff;
  margin: 0 20px;
  cursor: pointer;
  z-index: 5;
}
.swiper-menu .btn:nth-child(1)::before {
  content: "<";
}
.swiper-menu .btn:nth-child(2)::before {
  content: ">";
}
.swiperFram {
  position: absolute;
  left: 0;
  top: 0;
  height: 100%;
  float: left;
  transition: all 0.5s;
}
</style>
