<template>
  <div class="con-code">
    <div v-if="status == 2" class="content-delivery">
      <header>
        <h2>
          {{ title }}
        </h2>
      </header>
      <div class="con-qr">
        <img :src="qr" alt="">
      </div>
      <footer>
        <p>
          {{ text }}
        </p>
      </footer>
    </div>
    <div v-if="status == 1" class="content-delivery">
      <tracking />
    </div>
  </div>
</template>
<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'
import QRCode from 'qrcode'
@Component
export default class name extends Vue {
  qr: any = ''
  status: any = 1

  @Prop({}) token: any
  @Prop({}) title: any
  @Prop({}) text: any

  mounted() {
    QRCode.toDataURL(`${this.token}`, {
      margin: 2,
      scale: 10,
      color: {
        dark: '#000'
      }
    })
    .then(url => {
      this.qr = url
    })
    .catch(err => {
      console.error(err)
    })
  }
}
</script>
<style lang="sass" scoped>
.content-delivery
  flex: 1
  display: flex
  align-items: center
  justify-content: center
  flex-direction: column
.con-code
  width: 100%
  height: auto
  display: flex
  align-items: center
  justify-content: center
  flex-direction: column
  padding: 20px
  flex: 1
  p
    margin-top: 20px
    text-align: center
    font-size: .9rem
    opacity: .8
  .con-qr
    display: flex
    align-items: center
    justify-content: center
    padding: 20px
    width: 100%
    flex: 1
    img
      width: 100%
      max-width: 250px
      border-radius: 40px
// responsive

// @media (max-width: 812px), (pointer:none), (pointer:coarse)
</style>
