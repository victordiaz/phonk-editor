<template>
  <div id = "handle" v-bind:class = "[orientation, color]"> </div>
</template>

<script>

export default {
  name: 'Handle',
  props: {
    orientation: String,
    container: String,
    color: String
  },
  data () {
    return {
      handle: null
    }
  },
  mounted () {
    var that = this

    this.$nextTick(function () {
      that.handle = this.$el
      var container = document.querySelector('#' + this.container)
      // console.log(this.orientation + ' ' + handle + ' ' + this.container)

      that.handle.onmousedown = function (e) {
        var handlePos
        var containerSize
        // console.log(this.orientation)
        // console.log(e)

        if (that.orientation === 'vertical') {
          handlePos = that.handle.getBoundingClientRect().left
          containerSize = container.getBoundingClientRect().width
          // console.log(that.container + ' qq')
        } else {
          // console.log('qq2')
          handlePos = that.handle.getBoundingClientRect().top
          containerSize = container.getBoundingClientRect().height
        }

        document.onmousemove = function (e) {
          if (that.orientation === 'vertical') {
            // move handle
            var barW = that.handle.getBoundingClientRect().width
            var positionX = e.pageX - handlePos - barW / 2
            // console.log(handleX + ' ' + ' ' + ' ' + e.pageX + ' ' + position)

            // adjust container size
            container.style.width = containerSize - positionX + 'px'
          } else {
            // move handle
            var barH = that.handle.getBoundingClientRect().height
            var positionY = e.pageY - handlePos - barH / 2
            // console.log(handleX + ' ' + ' ' + ' ' + e.pageX + ' ' + position)

            // adjust container size
            container.style.height = containerSize + positionY + 'px'
          }
          e.preventDefault()
          e.stopPropagation()
        }

        document.onmouseup = function (e) {
          document.onmousemove = null
        }

        e.preventDefault()
        e.stopPropagation()
      }

      that.handle.onmouseup = function () {
        // console.log('handle mouse up')
        document.onmousemove = null
      }
    })
  },
  destroyed () {
    // Store.removeListener('toggle', this.toggle_section)
  }
}
</script>

<style lang='less'>
@import (reference) '../assets/css/variables.less';

#handle {
  &.vertical,
  &.horizontal {
    &.dark {
      background: transparent;
    }

    &:hover {
      cursor: col-resize;
      background: @accentColor;
    }

    &:active {
      background: @accentColor_1;
    }
  }

  &.vertical {
    width: 4px;
    height: calc(~'100%');
    right: 0px;
    top: 0px;
    margin-right: -4px;
    z-index: 2;

    &:hover {
      cursor: col-resize;
    }
  }

  &.horizontal {
    width: 100%;
    height: 4px;
    width: calc(~'100%');
    margin-top: -2px;

    &:hover {
      cursor: row-resize;
    }
  }
}
</style>
