<template>
  <button :type="type"
    @click="buttonClick($event)"
  >
    <slot></slot>
    <div ref="circle" class="circle"></div>
  </button>
</template>

<script>
export default {
  props: ['type'],
  methods: {
    buttonClick($event) {
      this.$emit('click', $event)

      if (process.browser) {
        this.animate($event)
      }
    },
    animate($event) {
      let circle = this.$refs.circle
      let self = this

      let opacityStep = (50 / ((self.$el.offsetWidth + 80) / 6)) / 100
      
      circle.style.opacity = 0.5
      circle.style.top = $event.offsetY + 'px'
      circle.style.left = $event.offsetX + 'px'

      let id = requestAnimationFrame(function circleTick() {
        let nextWidth = +circle.style.width.replace('px', '')

        if (nextWidth < self.$el.offsetWidth + 80) {
          nextWidth += 6

          circle.style.width = circle.style.height = nextWidth + 'px'
          circle.style.opacity = circle.style.opacity - opacityStep

          requestAnimationFrame(circleTick)
        } else {
          circle.style.width = circle.style.height = '0'
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
button {
  position: relative;
  width: 100px;
  height: 40px;

  overflow: hidden;
  cursor: pointer;

  background-color: #311b92;
  outline: none;
  border: 0;
  border-radius: 4px;

  color: white;
  font-size: 20px;
  font-family: 'Roboto', sans-serif;

  .circle {
    position: absolute;

    width: 0;
    height: 0;

    border-radius: 50%;
    background: white;
    opacity: 0;

    transform: translate(-50%, -50%);
  }
}
</style>