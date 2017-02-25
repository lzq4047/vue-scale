<template>
  <div class="scale">
    <div class="scale__actions">
      <span class="button-group">
        <button
          @click="enlarge(step)"
          @mousedown="handleButtonMouseDown(enlarge, $event)"
          @mouseup="handleButtonMouseUp($event)"
          :title="'长按' + quickScaleTimeout + 'ms快速放大'">
          <span class="fa fa-search-plus"></span>
        </button>
        <button
          @click="reduce(step)"
          @mousedown="handleButtonMouseDown(reduce, $event)"
          @mouseup="handleButtonMouseUp($event)"
          :title="'长按' + quickScaleTimeout + 'ms快速缩小'">
          <span class="fa fa-search-plus"></span>
        </button>
        <button
          @click="movemode = !movemode"
          :class="{active: movemode}">
          <span class="fa fa-arrows"></span>
        </button>
      </span>
    </div>
    <div class="scale__inner" ref="inner">
      <div
        @mousemove="handleMouseMove($event)"
        @mousedown="handleMouseDown($event)"
        @mouseup="handleMouseUp($event)"
        class="scale__content"
        :class="{move: movemode}"
        :style="contentStyle">
          <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'DScale',
    componentName: 'DScale',
    data: function () {
      return {
        scale: 1,
        movemode: false,
        beginmove: false,
        scrollTop: 0,
        scrollLeft: 0,
        lastPoint: {
          x: 0,
          y: 0
        },
        mouseMoveTimeout: null,
        longPressInterval: null,
        longPressTimeout: null
      }
    },
    props: {
      step: {
        type: Number,
        default: 0.1
      },
      quickScaleTimeout: {
        type: Number,
        default: 1000
      },
      quickScaleInterval: {
        type: Number,
        default: 100
      }
    },
    computed: {
      contentStyle: function () {
        return {
          transform: 'scale(' + this.scale + ')'
        }
      }
    },
    methods: {
      enlarge: function (step) {
        this.scale = this.scale + step
      },
      reduce: function (step) {
        this.scale = this.scale - step
      },
      handleMouseMove: function (e) {
        if (!this.beginmove) {
          return
        }
        if (this.mouseMoveTimeout) {
          return
        } else {
          this.mouseMoveTimeout = setTimeout(() => {
            clearTimeout(this.mouseMoveTimeout)
            this.mouseMoveTimeout = null
          }, 50)
        }
        let deltaX = e.screenX - this.lastPoint.x
        let deltaY = e.screenY - this.lastPoint.y
        this.scrollLeft = this.scrollLeft - deltaX
        this.scrollTop = this.scrollTop - deltaY
        if (this.scrollLeft < 0) {
          this.scrollLeft = 0
        } else if (this.scrollLeft > this.$refs.inner.scrollWidth) {
          this.scrollLeft = this.$refs.inner.scrollWidth
        }
        if (this.scrollTop < 0) {
          this.scrollTop = 0
        } else if (this.scrollTop > this.$refs.inner.scrollHeight) {
          this.scrollTop = this.$refs.inner.scrollHeight
        }
        this.lastPoint = {
          x: e.screenX,
          y: e.screenY
        }
        this.$refs.inner.scrollTop = this.scrollTop
        this.$refs.inner.scrollLeft = this.scrollLeft
      },
      handleMouseDown: function (e) {
        if (!this.movemode) {
          return
        }
        this.lastPoint = {
          x: e.screenX,
          y: e.screenY
        }
        this.beginmove = true
      },
      handleMouseUp: function (e) {
        this.beginmove = false
      },
      handleButtonMouseDown: function (cb, e) {
        this.longPressTimeout = setTimeout(() => {
          this.longPressInterval = setInterval(() => {
            cb(this.step)
          }, this.quickScaleInterval)
          clearTimeout(this.longPressTimeout)
        }, this.quickScaleTimeout)
      },
      handleButtonMouseUp: function (e) {
        if (this.longPressTimeout) {
          clearTimeout(this.longPressTimeout)
        }
        if (this.longPressInterval) {
          clearInterval(this.longPressInterval)
        }
      }
    },
    mounted: function () {
      this.$nextTick(() => {
      })
    }
  }
</script>

<style>
  .scale{
    position: relative;
    width: 100%;
    height: 100%;
    border: 1px solid #a2b0c4;
    overflow: hidden;
    background-color: #ebebeb;
    font-size: 12px;
  }
  .scale__actions{
    position: absolute;
    left: 10px;
    top: 10px;
    z-index: 1;
  }
  .scale__inner{
    width: 100%;
    height: 100%;
    overflow: auto;
  }
  .scale__inner::-webkit-scrollbar{
    display: none;
  }
  .scale__content{
    transform-origin: left top;
    height: 100%;
    width: 100%;
  }
  .scale__content.move{
    cursor: move;
  }
  .button-group{
    display: table;
    font-size: 0;
    border-collapse: collapse;
    background: #FFFFFF;
    border: 1px solid #EBEBEB;
    box-shadow: 2px 2px 4px 0px rgba(0,0,0,0.17);
    border-radius: 4px;
  }
  .button-group button{
    padding: 8px;
    display: table-cell;
    height: 30px;
    font-size: 12px;
    color: #47b2fb;
    background-color: transparent;
    border: 1px solid #ebebeb;
    outline: 0;
    cursor: pointer;
    box-shadow: none;
  }
  .button-group button:hover{
    border: 1px solid #47b2fb;
  }
  .button-group button.active{
    background-color: #47b2fb;
    color: #fff;
  }
  .button-group button + button{
    border-left: 1px solid #ebebeb;
  }

</style>
