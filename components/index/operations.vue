<template>
  <div
    id="operations"
    class="operations"
    :class="{ open: this.$route.query.operations}"
  >
    <header ref="header" @click="$emit('click')">
      <h2>
        Operaciones
      </h2>
      <i class='bx bxs-chevron-up'></i>
    </header>
    <div
      ref="btns" class="con-btns">
      <button
        :class="{ active : scrollLeft < windowInnerWidth - windowInnerWidth / 2 }"
        @click="handleClickBtn(0)"
      >
        Enviando <span v-if="filterOperations(1).length > 0" class="badge">{{ filterOperations(1).length }}</span>
      </button>
      <button
        :class="{ active : scrollLeft < windowInnerWidth * 2 - windowInnerWidth / 2 && scrollLeft > windowInnerWidth - windowInnerWidth / 2 }"
        @click="handleClickBtn(windowInnerWidth)"
      >
        Verificando <span v-if="filterOperations(2).length > 0" class="badge">{{ filterOperations(2).length }}</span>
      </button>
      <button
        :class="{ active : scrollLeft < windowInnerWidth * 3 - windowInnerWidth / 2 && scrollLeft > windowInnerWidth * 2 - windowInnerWidth / 2 }"
        @click="handleClickBtn(windowInnerWidth * 2)"
      >
        Recibiendo <span v-if="filterOperations(3).length > 0" class="badge">{{ filterOperations(3).length }}</span>
      </button>
      <button
        :class="{ active : scrollLeft < windowInnerWidth * 4 - 20 && scrollLeft > windowInnerWidth * 3 - windowInnerWidth / 2 }"
        @click="handleClickBtn(windowInnerWidth * 3)"
      >
        Finalizada <span v-if="filterOperations(4).length > 0" class="badge">{{ filterOperations(4).length }}</span>
      </button>
    </div>

    <!-- <pre>
      {{ operations }}
    </pre> -->

    <div ref="infos" class="con-infos">
      <div class="parent-info-2 parent-info">
        <template v-if="operations">
          <div
            v-for="(li, i) in filterOperations(1)"
            :key="i"
            @click="handleClickOperationPay(li)"
            class="info">
            <div class="data icons">
              <i v-if="li.type_operation_user_id == 3" class='bx bx-trip'></i>
              <i v-if="li.type_operation_user_id == 2" class='bx bx-buildings'></i>
              <i v-if="li.type_operation_user_id == 1" class='bx bx bx-transfer'></i>
            </div>
            <div class="data">
              <span>
                Enviar
              </span>
              <p>
                {{ li.send }}
              </p>
            </div>
            <div class="data">
              <span>
                Recibir
              </span>
              <p>
                {{ li.received }}
              </p>
            </div>
            <div class="data">
              <span>
                Fecha
              </span>
              <p>
                {{ `${li.datex.split('-')[2]}-${li.datex.split('-')[1]}-${li.datex.split('-')[0]}` }}
              </p>
            </div>
            <div class="data arrow">
              <i class='bx bxs-chevron-right'></i>
            </div>
          </div>

          <div class="not-found" v-if="filterOperations(1).length == 0">
            <p>
              No hay operaciones
            </p>
          </div>
        </template>

        <template v-else>
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
        </template>
      </div>
      <div class="parent-info-1 parent-info">
        <template v-if="operations">
          <div
            v-for="(li, i) in filterOperations(2)"
            :key="i"
            @click="handleClickOperation(li)"
            class="info">
            <div class="data icons">
              <i v-if="li.type_operation_user_id == 3" class='bx bx-trip'></i>
              <i v-if="li.type_operation_user_id == 2" class='bx bx-buildings'></i>
              <i v-if="li.type_operation_user_id == 1" class='bx bx bx-transfer'></i>
            </div>
            <div class="data">
              <span>
                Enviar
              </span>
              <p>
                {{ li.send }}
              </p>
            </div>
            <div class="data">
              <span>
                Recibir
              </span>
              <p>
                {{ li.received }}
              </p>
            </div>
            <div class="data">
              <span>
                Fecha
              </span>
              <p>
                {{ `${li.datex.split('-')[2]}-${li.datex.split('-')[1]}-${li.datex.split('-')[0]}` }}
              </p>
            </div>
            <div class="data arrow">
              <i class='bx bxs-chevron-right'></i>
            </div>
          </div>
          <div class="not-found" v-if="filterOperations(2).length == 0">
            <p>
              No hay operaciones
            </p>
          </div>
        </template>
        <template v-else>
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
        </template>
      </div>
      <div class="parent-info-3 parent-info">
        <template v-if="operations">
          <div
            v-for="(li, i) in filterOperations(3)"
            :key="i"
            @click="handleClickOperationReceive(li)"
            class="info">
            <div class="data icons">
              <i v-if="li.type_operation_ekambia_id == 3" class='bx bx-trip'></i>
              <i v-if="li.type_operation_ekambia_id == 2" class='bx bx-buildings'></i>
              <i v-if="li.type_operation_ekambia_id == 1" class='bx bx bx-transfer'></i>
            </div>
            <div class="data">
              <span>
                Enviar
              </span>
              <p>
                {{ li.send }}
              </p>
            </div>
            <div class="data">
              <span>
                Recibir
              </span>
              <p>
                {{ li.received }}
              </p>
            </div>
            <div class="data">
              <span>
                Fecha
              </span>
              <p>
                {{ `${li.datex.split('-')[2]}-${li.datex.split('-')[1]}-${li.datex.split('-')[0]}` }}
              </p>
            </div>
            <div class="data arrow">
              <i class='bx bxs-chevron-right'></i>
            </div>
          </div>

          <div class="not-found" v-if="filterOperations(3).length == 0">
            <p>
              No hay operaciones
            </p>
          </div>
        </template>

        <template v-else>
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
        </template>
      </div>
      <div class="parent-info-4 parent-info">
        <template v-if="operations">
          <div
            v-for="(li, i) in filterOperations(4)"
            :key="i"
            @click="handleClickOperationFinish(li)"
            class="info">
            <div class="data icons">
              <i class='bx bx-check' ></i>
            </div>
            <div class="data">
              <span>
                Enviar
              </span>
              <p>
                {{ li.send }}
              </p>
            </div>
            <div class="data">
              <span>
                Recibir
              </span>
              <p>
                {{ li.received }}
              </p>
            </div>
            <div class="data">
              <span>
                Fecha
              </span>
              <p>
                {{ `${li.datex.split('-')[2]}-${li.datex.split('-')[1]}-${li.datex.split('-')[0]}` }}
              </p>
            </div>
            <div class="data arrow">
              <i class='bx bxs-chevron-right'></i>
            </div>
          </div>

          <div class="not-found" v-if="filterOperations(4).length == 0">
            <p>
              No hay operaciones
            </p>
          </div>
        </template>

        <template v-else>
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
          <load block height="61px" style="margin-bottom: 10px" />
        </template>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'
import axios from '~/plugins/axios'
import { State, Action, Mutation } from 'vuex-class'
@Component({
  fetch() {
    (this as any).getOperations()
  },
  fetchOnServer: false
})
export default class OperationsClass extends Vue {
  seg: number = 0
  secFun: any = null
  scrollLeft: number = 0
  windowInnerWidth: number = 0

  @Prop({}) scrollTop: number

  @State(state => state.operations.operations) operations

  @Action('operations/getOperations') getOperations

  @Mutation('steps/SET_DATA') setStepData

  handleClickOperationReceive(operation) {
    this.setStepData(operation)
    if (operation.type_operation_ekambia_id == 1) {
      this.$router.push({
        path: '/step3/',
        query: {
          source: 'operations',
          id: operation.id,
          verified: 'true'
        }
      })
    } else if (operation.type_operation_ekambia_id == 3) {
      this.$router.push({
        path: '/step3/delivery/',
        query: {
          source: 'operations',
          id: operation.id
        }
      })
    } else if (operation.type_operation_ekambia_id == 2) {
      if (operation.step_out_id == 2) {
        this.$router.push({
          path: '/step3/office/',
          query: {
            source: 'operations',
            id: operation.id,
            step: '2'
          }
        })
      } else {
        this.$router.push({
          path: '/step3/office/',
          query: {
            source: 'operations',
            id: operation.id
          }
        })
      }
    }
  }

  handleClickOperation(operation) {
    this.setStepData(operation)
    if (operation.type_operation_ekambia_id == 1) {
      this.$router.push({
        path: '/step3/',
        query: {
          source: 'operations',
          id: operation.id
        }
      })
    } else if (operation.type_operation_ekambia_id == 3) {
      this.$router.push({
        path: '/step3/delivery/',
        query: {
          source: 'operations',
          id: operation.id,
          checking: `${operation.status_operation_id == 2}`
        }
      })
    } else if (operation.type_operation_ekambia_id == 2) {
      this.$router.push({
        path: '/step3/office/',
        query: {
          source: 'operations',
          id: operation.id,
          checking: `${operation.status_operation_id == 2 && operation.type_operation_user_id == 1}`
        }
      })
    }
  }

  handleClickOperationPay(operation) {
    this.setStepData(operation)
    if (operation.type_operation_user_id == 1) {
      this.$router.push({
        path: '/step2/',
        query: {
          source: 'operations',
          id: operation.id
        }
      })
    } else if (operation.type_operation_user_id == 2) {
      if (operation.step_in_id == 2) {
        this.$router.push({
          path: '/step2/office/',
          query: {
            source: 'operations',
            id: operation.id,
            step: '2'
          }
        })
      } else {
        this.$router.push({
          path: '/step2/office/',
          query: {
            source: 'operations',
            id: operation.id
          }
        })
      }
    } else if (operation.type_operation_user_id == 3) {
      if (operation.status_location_delivery_in_id == 2) {
        this.$router.push({
          path: '/step2/delivery/',
          query: {
            source: 'operations',
            id: operation.id,
            transit: 'true'
          }
        })
      } else if (operation.status_location_delivery_in_id == 3) {
        this.$router.push({
          path: '/step2/delivery/',
          query: {
            id: operation.id,
            qr: 'true'
          }
        })
      } else {
        this.$router.push({
          path: '/step2/delivery/',
          query: {
            source: 'operations',
            id: operation.id
          }
        })
      }
    }
  }

  handleClickOperationFinish(operation) {
    this.setStepData(operation)
    this.$router.push({
      path: '/step4',
      query: {
        source: 'operations',
        id: operation.id
      }
    })
  }

  filterOperations(id) {
    const operations = this.operations || []
    return operations.filter((item) => {
      return item.status_operation_id == id
    })
  }

  handleSeg () {
    this.seg++
  }

  handleStart () {
    this.secFun = setInterval(this.handleSeg, 100)
  }

  handleEnd () {
    if (this.seg < 4) {
      this.$emit('touchend', this.$el)
    }
    clearInterval(this.secFun)
    this.seg = 0
  }

  handleClickBtn (left: number) {
    this.windowInnerWidth = (this.$refs.infos as any).offsetWidth
    const infos: any = this.$refs.infos
    const btns: any = this.$refs.btns
    btns.classList.add('scroll')
    setTimeout(() => {
      btns.classList.remove('scroll')
    }, 300);
    infos.scrollTo(left, 0)
  }

  mounted () {
    setTimeout(() => {
      this.windowInnerWidth = (this.$refs.infos as any).offsetWidth
    }, 300);

    const infos: any = this.$refs.infos
    infos.addEventListener('scroll', (evt) => {
      this.scrollLeft = evt.target.scrollLeft;

      const btns: any = this.$refs.btns

      if (btns.scrollLeft !== this.scrollLeft / 4) {
        btns.scrollTo(this.scrollLeft / 5, 0)
      }

      // if (this.scrollLeft > this.windowInnerWidth * 2 - this.windowInnerWidth / 2 ) {
      //   (this.$refs.btns as any).scrollTo(this.windowInnerWidth * 2, 0)
      // }
    })

  }
}
</script>
<style lang="sass" scoped>
.not-found
  width: 100%
  text-align: center
  p
    font-size: .9rem
    padding: 15px
.operations
  position: relative
  width: 100%
  scroll-snap-align: center
  background: #fff
  height: 80vh
  background: -color('gray')
  border-radius: 30px 30px 0px 0px
  padding-bottom: 75px
  min-height: 500px
  &.open
    header
      i
        transform: rotate(180deg)
  .con-infos
    display: flex
    align-items: flex-start
    justify-self: flex-start
    overflow: auto
    scroll-snap-type: x mandatory
    scroll-behavior: smooth
    .parent-info
      padding: 15px
      padding-bottom: 90px
      overflow: auto
      max-height: calc(80vh - 150px)
      min-height: calc(80vh - 150px)
      min-width: 100%
      scroll-snap-align: center
    .info
      display: flex
      align-items: center
      justify-content: space-between
      background: -color('bg')
      border-radius: 20px
      margin-bottom: 10px
      cursor: pointer
      transition: all .25s ease
      &:hover
        background: -color(gray)
      .data
        padding: 14px 10px
        font-size: .8rem
        display: flex
        align-items: flex-start
        justify-content: center
        flex-direction: column
        position: relative
        width: 33%
        &.icons
          min-width: 40px
          width: 40px
          padding: 0px
          display: flex
          align-items: center
          justify-content: center
          i
            font-size: 1.2rem
        &.arrow
          width: 40px
          display: flex
          align-items: center
          justify-content: center
          opacity: .6
          i
            font-size: 1.1rem
        &:first-child
          &:after
            content: ''
            position: absolute
            right: 0px
            width: 2px
            height: 25px
            background: -color('black', .05)
        &:nth-child(3),&:nth-child(4)
          &:after
            content: ''
            position: absolute
            left: 0px
            width: 2px
            height: 25px
            background: -color('black', .05)
        span
          opacity: .5
          font-size: .7rem
        p
          font-weight: 500
  .con-btns
    display: flex
    align-items: center
    justify-content: space-between
    width: 100%
    margin: 10px 0px
    padding: 5px
    padding-left: 15px
    margin-bottom: 0px
    margin-top: 0px
    position: relative
    z-index: 20
    overflow: auto
    &.scroll
      scroll-behavior: smooth
    &::after
      content: ''
      position: relative
      min-width: 2px
      height: 5px
    button
      padding: 12px 22px
      border-radius: 18px
      border: 0px
      background: transparent
      transition: all .25s ease
      position: relative
      margin-right: 12px
      background: rgba(0,0,0,.03)
      &.active
        background: -color('black')
        color: -color('bg')
      span
        position: absolute
        top: -6px
        background: #fff
        color: #000
        min-width: 20px
        border-radius: 7px
        font-weight: bold
        font-size: .75rem
        padding: 2px 0px
        right: -4px
  header
    border-radius: 30px 30px 0px 0px
    position: relative
    background: -color('gray')
    border-radius: 30px 30px 0px 0px
    display: flex
    align-items: center
    justify-content: space-between
    padding: 20px 30px
    h2
      font-size: 1rem
      font-weight: normal
    i
      transition: all .25s ease
      font-size: 1.1rem
// responsive

@media (max-width: 812px)
  .con-btns
    button
      min-width: 35%
@media (min-width: 812px)
  .operations
    padding-bottom: 0px
    display: flex
    align-items: center
    justify-content: center
    flex-direction: column
    header
      width: 100%
    .con-infos
      max-width: 850px
      width: 100%
      height: calc(80vh - 114px)
      .parent-info
        padding-bottom: 20px
        width: 100%
    .con-btns
      max-width: 850px
      margin: 0px auto
      button
        width: 25%
        cursor: pointer
</style>
