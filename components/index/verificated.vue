<template>
  <div
    class="vefificated"
    :class="{
      animate: this.$route.query.animate
    }"
  >
    <div ref="con" class="con">
      <div class="con-circle">
        <div class="con-send con-icon">
          <div class="icon">
            <img src="/logo1.svg" alt="">
          </div>
          <span v-if="data">
            {{ data.send }} {{ data.coin_send.coin }}
          </span>
        </div>
        <div class="con-receive con-icon">
          <div class="icon">
            <i v-if="this.$route.query.animate" class='bx bx-check'></i>
            <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 22 25">
              <g id="Grupo_2254" data-name="Grupo 2254" transform="translate(-196 -819)">
                <path id="Sustracción_1" data-name="Sustracción 1" d="M-5318.055-15h-18.334a.618.618,0,0,1-.611-.624v-2.5a.619.619,0,0,1,.611-.626h1.833V-27.5h-1.833a.619.619,0,0,1-.611-.625v-2.288a1.247,1.247,0,0,1,.758-1.149l7.822-3.251a2.39,2.39,0,0,1,.928-.188h.558a2.386,2.386,0,0,1,.933.188l7.8,3.251a1.247,1.247,0,0,1,.758,1.149v2.288a.619.619,0,0,1-.611.625h-1.834v8.749h1.834a.619.619,0,0,1,.611.626v2.5A.618.618,0,0,1-5318.055-15ZM-5326-27.5v8.749h3.667V-27.5Zm-6.11,0v8.749h3.667V-27.5Z" transform="translate(5534.223 854)"/>
                <path id="Path" d="M21.389,0H.611A.618.618,0,0,0,0,.625v1.25A.618.618,0,0,0,.611,2.5H21.389A.618.618,0,0,0,22,1.875V.625A.618.618,0,0,0,21.389,0Z" transform="translate(196 841.5)" opacity="0.3"/>
              </g>
            </svg>
          </div>
          <span v-if="data">
            {{ data.received }} {{ data.coin_received.coin }}
          </span>
        </div>
        <div class="circle">
          <div class="spinner-box">
            <div class="circle-border">
              <div class="circle-core"></div>
            </div>
          </div>
        </div>
        <!-- <svg class="svg-planeta" viewBox="0 0 160 160" width="160" height="160">
          <g transform=" matrix(0.866, -0.5, 0.25, 0.433, 80, 80)">
            <path d="M 0,70 A 65,70 0 0,0 65,0 5,5 0 0,1 75,0 75,70 0 0,1 0,70Z" fill="#000">
              <animateTransform attributeName="transform" type="rotate" from="360 0 0" to="0 0 0" dur="1s" repeatCount="indefinite" />
            </path>
          </g>
        </svg> -->
        <img src="planeta.png" alt="">
      </div>
    </div>
    <p v-if="this.$route.query.animate">
      Operación verificada con éxito, transferencia efectuada a su cuenta
    </p>
    <p v-else>
      Se esta verificando la transacción espere un momento...
    </p>

    <div :class="{active: this.$route.query.animate}" class="con-btn-success">
      <Button @click="$router.push('/')" block yellow>
        Aceptar
      </Button>
    </div>
  </div>
</template>
<script lang="ts">
import { Component, Vue, Prop, Watch } from 'vue-property-decorator'
import axios from '~/plugins/axios'
@Component
export default class vefificated extends Vue {
  @Prop() open: boolean
  @Prop() black: boolean

  data: any = null
  animate: boolean = false

  getOperation() {
    axios.get(`/operation-show/${this.$route.query.id}`).then(({data}: any) => {
      console.log(data.info)
      this.data = data.info
    })
  }

  @Watch('$route.query.animate')
  handleAnimate() {
    this.animate = true
  }

  mounted() {
    this.getOperation()
  }
}
</script>
<style>
.spinner-box {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: transparent;
  position: absolute;
}


</style>
<style lang="sass" scoped>
.con-btn-success
  position: absolute
  bottom: 0px
  width: 100%
  padding: 20px
  margin-bottom: 120px
  z-index: 200
  transform: translate(0,100%)
  opacity: 0
  transition: all .5s ease
  visibility: hidden
  &.active
    visibility: visible
    opacity: 1
    transform: translate(0)
.circle-core
  width: 100%
  height: 100%
  background-color: -color(gray)
  border-radius: 50%

.circle-border
  width: 100%
  height: 100%
  padding: 3px
  display: flex
  justify-content: center
  align-items: center
  border-radius: 50%
  background: rgb(63,249,220)
  background: linear-gradient(0deg, -color(gray) 33%, -color(color) 100%)
  animation: spin .8s linear 0s infinite

.svg-planeta
  position: absolute
  z-index: 100
.vefificated
  background: -color('gray')
  width: 100%
  border-radius: 30px 30px 0px 0px
  // transition: all .25s ease
  display: flex
  align-items: center
  justify-content: center
  flex-direction: column
  flex: 1
  &.animate
    .con-circle
      .circle
        width: 550px
        height: 550px
        .circle-border
          animation: none
          background: -color(color)
          box-shadow: 0px 10px 40px 0px -color('color', .25)
    .con-circle
      width: 60px
      height: 60px
      .con-icon
        &.con-receive
          span
            bottom: -40px
        span
          top: -40px
        .icon
          transform: scale(1.2)
  p
    padding: 20px
    text-align: center
  &.black
    background: -color('black')
    .con
      opacity: 0
    header
      color: -color('bg')
  &.open
    background: -color('gray')
    header
      color: -color('text')
  .con
    // transition: all .25s ease
    display: flex
    align-items: center
    justify-content: center
    flex-direction: column
    width: 100%
    max-width: 600px
    flex: 1
  header
    padding: 20px
    width: 100%
    max-width: 600px
    h3
      font-weight: 600
      font-size: 1rem
      b
        font-weight: bold
        font-size: 1.1rem
  .con-circle
    position: relative
    display: flex
    align-items: center
    justify-content: center
    width: 260px
    height: 260px
    z-index: 10
    transition: all .7s ease
    .con-icon
      position: absolute
      z-index: 100
      display: flex
      align-items: center
      justify-content: center
      i
        font-size: 2.8rem
      &.con-send
        top: -5px
        left: -5px
        z-index: 110
      &.con-receive
        right: -5px
        bottom: -5px
        z-index: 110
        span
          top: auto
          bottom: -15px
          transform: translate(0, 100%)
      span
        position: absolute
        top: -12px
        transform: translate(0,-100%)
        font-weight: bold
        font-size: .9rem
        text-align: center
        transition: all 1s ease
      .icon
        background: -color('color')
        width: 70px
        height: 70px
        border-radius: 24px
        display: flex
        align-items: center
        justify-content: center
        box-shadow: 0px 10px 20px 0px -color('color', .4)
        z-index: 110
        position: relative
        svg
          width: 30px
        img
          width: 45px
    img
      width: 140px
    .circle
      border-radius: 50%
      // border: 2px dashed -color('black', .3)
      width: 260px
      height: 260px
      display: flex
      align-items: center
      justify-content: center
      position: absolute
      z-index: -10
      transition: all 1s ease
  p
    opacity: .7
    font-size: .8rem
    padding: 30px 25px

@keyframes rotate
  0%
    transform: rotate(-43deg)
  100%
    transform: rotate(143deg)
// responsive

// @media (max-width: 812px), (pointer:none), (pointer:coarse)
@media (min-width: 812px)
  .vefificated
    padding-bottom: 0px
    margin-bottom: 0px
    border-radius: 30px
    header
      border-radius: 30px
</style>
