<template>
  <div class="user page">
    <nav-bar v-if="$device.isMobile" absolute />
    <h2>
      Mi Perfil
    </h2>

    <div class="con-infos">
      <div
        :class="{
          open: open == 1
        }"
        class="con-info">
        <header
          @click="open === 1 ? open = 0 : open = 1"
        >
          <div class="left">
            <div class="con-icon">
              <i class='bx bxs-user-circle'></i>
            </div>
            <h3>
              Datos personales
            </h3>
          </div>
          <div class="right">
            <i class='bx bxs-chevron-right'></i>
          </div>
        </header>
        <transition
          @before-enter="beforeEnter"
          @enter="enter"
          @leave="leave"
        >
          <div v-if="open == 1" class="content-info">
            <personal-data @cancel="cancelData" />
          </div>
        </transition>
      </div>
      <div
        :class="{
          open: open == 2
        }"
        class="con-info">
        <header
          @click="open === 2 ? open = 0 : open = 2"
        >
          <div class="left">
            <div class="con-icon">
              <i class='bx bx-donate-heart'></i>
            </div>
            <h3>
              Recomienda y Gana
            </h3>
          </div>
          <div class="right">
            <i class='bx bxs-chevron-right'></i>
          </div>
        </header>
        <transition
          @before-enter="beforeEnter"
          @enter="enter"
          @leave="leave"
        >
          <div v-if="open == 2" class="content-info">
            <win />
          </div>
        </transition>
      </div>
      <div
        :class="{
          open: open == 3
        }"
        class="con-info">
        <header
          @click="open === 3 ? open = 0 : open = 3"
        >
          <div class="left">
            <div class="con-icon">
              <i class='bx bxs-bell' ></i>
            </div>
            <h3>
              Alertas y tipo de cambio
            </h3>
          </div>
          <div class="right">
            <i class='bx bxs-chevron-right'></i>
          </div>
        </header>
        <transition
          @before-enter="beforeEnter"
          @enter="enter"
          @leave="leave"
        >
          <div v-show="open == 3" class="content-info">
            <user-alerts />
          </div>
        </transition>
      </div>
      <div
        :class="{
          open: open == 4
        }"
        class="con-info">
        <header
          @click="open === 4 ? open = 0 : open = 4"
        >
          <div class="left">
            <div class="con-icon">
              <i class='bx bx-book' ></i>
            </div>
            <h3>
              Términos y condiciones
            </h3>
          </div>
          <div class="right">
            <i class='bx bxs-chevron-right'></i>
          </div>
        </header>
        <transition
          @before-enter="beforeEnter"
          @enter="enter"
          @leave="leave"
        >
          <div v-show="open == 4" class="content-info terms">
            <terms />
          </div>
        </transition>
      </div>
      <div
        @click="handleClickLogOut"
        class="con-info">
        <header>
          <div class="left">
            <div class="con-icon">
              <i class='bx bxs-door-open' ></i>
            </div>
            <h3>
              Cerrar Sesión
            </h3>
          </div>
          <div class="right">
          </div>
        </header>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import axios from '~/plugins/axios'
import { State, Action } from 'vuex-class'
@Component({
  layout: ({ isMobile }) => isMobile ? 'mobile' : 'default',
  fetch() {
    (this as any).getUserData(this.$route.query.id)
  },
  fetchOnServer: false
})
export default class name extends Vue {
  open: number = 0

  @State(state => state.user.data) data

  @Action('user/getUserData') getUserData

  @Action('user/updateUserData') updateUserData

  cancelData() {
    this.getUserData(this.$route.query.id)
  }

  handleClickLogOut () {
    this.$dialog({
      title: '¿Quieres cerrar la sesión?',
      bounce: true,
      success: () => {
        axios.post('/logout').then(() => {
          this.$cookies.set('token', '')
          this.$cookies.set('authenticated', false)
          this.$nextTick(() => {
            this.$router.push('/login')
          })
        }).catch((err) => {
          this.$notification({
            title: 'Oops! Algo salió mal',
            text: err.response.data.message.toString()
          })
        })
      }
    })
  }

  beforeEnter (el: any) {
    el.style.height = 0
  }

  enter (el: any, done: any) {
    const h = el.scrollHeight
    el.style.height = h - 1 + 'px'
    setTimeout(() => {
      el.style.height = 'auto'
    }, 300);
    done()
  }

  leave (el: any) {
    const h = el.scrollHeight
    el.style.height = h - 1 + 'px'
    setTimeout(() => {
      el.style.height = '0px'
    }, 10);
  }
}
</script>
<style lang="sass" scoped>
.user
  height: calc(var(--vh, 1vh) * 100)
  padding: 15px 10px
  padding-top: 50px
  padding-bottom: 80px
  background: -color('gray')
  display: flex
  align-items: flex-start
  justify-content: flex-start
  flex-direction: column
  overflow: auto
  width: 100%
  h2
    font-size: 1.2rem
    padding-left: 5px
  .con-infos
    width: 100%
    height: auto
    border-radius: 20px
    margin-top: 20px
    max-width: 500px
    padding-bottom: 60px
    h3
      font-size: 1rem
  .con-info
    background: -color('bg')
    width: 100%
    transition: all .25s ease
    &:first-child
     border-radius: 26px 26px 0px 0px
    &:last-child
     border-radius: 0px 0px 26px 26px
    &.open
      header
        .right
          transform: rotate(-90deg)
    header
      padding: 12px 15px
      display: flex
      align-items: center
      justify-content: space-between
      cursor: pointer
      .right
        transition: all .25s ease
        font-size: 1.3rem
      .left
        display: flex
        align-items: center
        justify-content: flex-start
        .con-icon
          border-radius: 14px
          width: 40px
          height: 40px
          background: -color('gray')
          display: flex
          align-items: center
          justify-content: center
          font-size: 1.3rem
          margin-right: 10px
    .content-info
      overflow: hidden
      transition: all .25s ease
      padding: 0px 15px
  .terms
    font-size: .7rem
    p
      padding-bottom: 20px
// responsive

@media (min-width: 812px)
  .user
    display: flex
    align-items: center
    justify-content: flex-start
    flex-direction: column
    .con-infos
      width: 100%
      max-width: 550px

</style>
