<template>
  <div id="carousel">
    <div class="window" @mouseover="stop" @mouseleave="play">
      <ul class="container" :style="containerStyle">
        <li>
          <!-- 把最后一张图放在最开始 -->
          <img :style="{width:imgWidth+'px'}" :src="sliders[sliders.length - 1].img" alt="">
        </li>
        <li  v-for="(item, index) in sliders" :key="index">
          <img :style="{width:imgWidth+'px'}" :src="item.img" alt="" @click="amplification(index)">
        </li>
        <li>
          <!-- 把第一张图放在最后 -->
          <img :style="{width:imgWidth+'px'}" :src="sliders[0].img" alt="">
        </li>
      </ul>
      <ul class="direction" v-if="indicator">
        <li class="left" @click="move(imgWidth, 1, speed)">
          <svg class="icon" width="30px" height="30.00px" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"><path fill="#ffffff" d="M481.233 904c8.189 0 16.379-3.124 22.628-9.372 12.496-12.497 12.496-32.759 0-45.256L166.488 512l337.373-337.373c12.496-12.497 12.496-32.758 0-45.255-12.498-12.497-32.758-12.497-45.256 0l-360 360c-12.496 12.497-12.496 32.758 0 45.255l360 360c6.249 6.249 14.439 9.373 22.628 9.373z"  /></svg>
        </li>
        <li class="right" @click="move(imgWidth, -1, speed)">
          <svg class="icon" width="30px" height="30.00px" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"><path fill="#ffffff" d="M557.179 904c-8.189 0-16.379-3.124-22.628-9.372-12.496-12.497-12.496-32.759 0-45.256L871.924 512 534.551 174.627c-12.496-12.497-12.496-32.758 0-45.255 12.498-12.497 32.758-12.497 45.256 0l360 360c12.496 12.497 12.496 32.758 0 45.255l-360 360c-6.249 6.249-14.439 9.373-22.628 9.373z"  /></svg>
        </li>
      </ul>
      <ul class="dots" v-if="point">
        <li v-for="(dot, i) in sliders" :key="i"
        :class="{dotted: i === (currentIndex-1)}"
        @click = jump(i+1)
        >
        </li>
      </ul>
    </div>
    <div class="mask-div" @click="maskFun()" v-if="maskBol">
      <img :style="{width:imgWidth+'px'}" :src="sliders[imgNumber].img" alt="" class="mask-img">
    </div>
  </div>
</template>
<script>
export default {
  name: 'carousel',
  props: {
    initialSpeed: {
      type: Number,
      default: 20
    },
    // 图片移动速度
    initialInterval: {
      type: Number,
      default: 1
    },
    // 是否需要指示器
    indicator: {
      type: Boolean,
      default: true
    },
    // 圆点显示
    point: {
      type: Boolean,
      default: true
    },
    // 图片的宽
    imgWidth: {
      type: Number,
      default: 800
    },
    // 图片数据
    sliders: {
      type: Array,
      default: () => [
        {
          img: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fcdn.duitang.com%2Fuploads%2Fitem%2F201202%2F18%2F20120218194349_ZHW5V.thumb.700_0.jpg&refer=http%3A%2F%2Fcdn.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1623571283&t=404c98966db279854a9bedc359f5fc1a'
        },
        {
          img: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fcdn.duitang.com%2Fuploads%2Fitem%2F201410%2F04%2F20141004172507_J8Mty.jpeg&refer=http%3A%2F%2Fcdn.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1623571337&t=bfe06e92fd14bc3f2faee4c8a913e5d3'
        },
        {
          img: 'https://ss0.baidu.com/94o3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/b151f8198618367a021b7f8b2e738bd4b21ce5d2.jpg'
        },
        {
          img: 'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=4152737816,990173897&fm=224&gp=0.jpg'
        }
      ]
    }
  },
  data () {
    return {
      currentIndex: 1, // 原点起始位置
      transitionEnd: true, // 防止多次快速点击切换出现问题的闸门
      speed: this.initialSpeed,
      distance: -800, // 设置图片移动距离（负数）
      timer: null, // 定时器
      imgNumber: 0, // 点击放大的图片
      maskBol: false
    }
  },
  computed: {
    containerStyle () {
      return {
        transform: `translate3d(${this.distance}px, 0, 0)`
      }
    },
    interval () {
      return this.initialInterval * 1000
    }
  },
  created () {
    this.init()
  },
  methods: {
    init () {
      this.play()
    },
    // 移动一次的距离， 向左还是向右移动， 图片移动速度
    move (offset, direction, speed) {
      if (!this.transitionEnd) return
      this.transitionEnd = false
      direction === -1 ? this.currentIndex += offset / this.imgWidth : this.currentIndex -= offset / this.imgWidth
      if (this.currentIndex > this.sliders.length) this.currentIndex = 1
      if (this.currentIndex < 1) this.currentIndex = this.sliders.length

      const destination = this.distance + offset * direction
      this.animate(destination, direction, speed)
    },
    // 实际移动距离 想左还是向右 移动速度 负右正左
    animate (des, direc, speed) {
      if (this.temp) {
        window.clearInterval(this.temp)
        this.temp = null
      }
      this.temp = window.setInterval(() => {
        if ((direc === -1 && des < this.distance) || (direc === 1 && des > this.distance)) {
          this.distance += speed * direc
        } else {
          this.transitionEnd = true
          window.clearInterval(this.temp)
          this.distance = des
          const allWidth = this.sliders.length * this.imgWidth
          if (des < -allWidth) this.distance = -this.imgWidth
          if (des > -this.imgWidth) this.distance = -allWidth
        }
      }, 10)
    },
    jump (index) {
      const direction = index - this.currentIndex >= 0 ? -1 : 1
      const offset = Math.abs(index - this.currentIndex) * this.imgWidth
      const jumpSpeed = Math.abs(index - this.currentIndex) === 0 ? this.speed : Math.abs(index - this.currentIndex) * this.speed
      this.move(offset, direction, jumpSpeed)
    },
    // 自动播放函数
    play () {
      if (!this.maskBol) {
        if (this.timer) {
          window.clearInterval(this.timer)
          this.timer = null
        }
        this.timer = window.setInterval(() => {
          this.move(this.imgWidth, -1, this.speed)
        }, this.interval)
      }
    },
    stop () {
      window.clearInterval(this.timer)
      this.timer = null
    },
    amplification (index) {
      this.imgNumber = index
      this.maskBol = true
      this.stop()
    },
    maskFun () {
      this.maskBol = false
      this.play()
    }
  }
}
</script>
<style lang="scss">
*{
    box-sizing: border-box;
    margin:0;
    padding:0;
  }
  ol,ul{
    list-style: none;
  }
  #carousel{
    text-align: center;
    padding: 15px 0;
    .window{
      position:relative;
      width:800px;
      height:300px;
      margin:0 auto;
      overflow:hidden;
      .container{
        display:flex;
        position:absolute;
        li {
            overflow: hidden;
        }
      }
      .left, .right{
        position:absolute;
        top:50%;
        transform:translateY(-50%);
        width:50px;
        height:50px;
        background-color:rgba(0,0,0,.3);
        border-radius:50%;
        cursor:pointer;
      }
      .left{
        left:3%;
        padding-left:12px;
        padding-top:10px;
      }
      .right{
        right:3%;
        padding-right:12px;
        padding-top:10px;
      }
      img{
        user-select: none;
      }
    }
    .dots{
        position:absolute;
        bottom:10px;
        left:50%;
        transform:translateX(-50%);
      }
    .dots li{
      display:inline-block;
      width:15px;
      height:10px;
      margin:0 3px;
      border:1px solid white;
      border-radius:30px;
      background-color:#333;
      cursor:pointer;
    }
    .dots .dotted{
      background-color:orange;
      width:30px;
    }
    .mask-div{
      width: 100vw;
      height: 100vh;
      background-color: rgba(0,0,0,.7);
      position: fixed;
      top: 0;
      left: 0;
      .mask-img{
        margin-top: calc(50vh - 200px);
      }
    }
  }
</style>
