<template>
  <div
    class="transfer">
    <div v-if="!transferred" class="con-data">
      <div class="card-texts">
        <div class="texts">
          <div class="text">
            <span>
              Tu envías
            </span>
            <p v-if="data">
              {{ data.send }} {{ data.coin_send.coin }}
            </p>
            <p v-else>
              ...
            </p>
          </div>
          <div class="text">
            <span>
              Tu recibes
            </span>
            <p v-if="data">
              {{ data.received }} {{ data.coin_received.coin }}
            </p>
            <p v-else>
              ...
            </p>
          </div>
        </div>
        <div class="textw">
          <span>
            Tipo de cambio utilizado
          </span>
          <p>
            12.123
          </p>
          <!-- <p>
            0.00
          </p> -->
        </div>
      </div>

      <Select child="name" :data="ekambiaAccounts" @change="handleChange" placeholder="Selecciona la cuenta" class="mt-6" block>
        <template v-if="ekambiaAccounts">
          <Option :key="i" v-for="(option, i) in ekambiaAccounts" :value="option.id" :text="option.name" :obj="option" />
        </template>
      </Select>

      <h2 v-if="account">
        Datos de cuenta bancaria
      </h2>
      <ul v-if="account">
        <li>
          <b>
            IBAN:
          </b>
          <p>
            {{ account.account_number }}
          </p>
        </li>
        <li>
          <b>
            Banco:
          </b>
          <p>
            {{ account.bank.name }}
          </p>
        </li>
        <li>
          <b>
            Nombre:
          </b>
          <p>
            {{ account.holder }}
          </p>
        </li>
      </ul>

      <footer v-if="account">
        <Button block yellow @click="transferred = true">
          Listo ya transferí
        </Button>
      </footer>
    </div>
    <div v-else class="boucher">
      <h2>
        Agregar comprobante de transferencia
      </h2>
      <div class="con">
        <input-file :danger="!form.ref && !form.file && send" :disabled="!!form.ref" v-model="form.file">
          Comprobante transferencia bancaria

          <template #text>
            Tome o suba <br> imagen de comprobante
          </template>
        </input-file>
        <Alert :open="!form.ref && !form.file && send">
          Este campo es requerido
        </Alert>
        <divider>
          O
        </divider>
        <c-input
          v-model="form.ref"
          class="mt-6"
          type="number"
          gray
          :disabled="!!form.file"
          :danger="!form.ref && !form.file && send"
          maxlength="20"
        >
          Nro. de referencia
        </c-input>
        <Alert :open="!form.ref && !form.file && send">
          Este campo es requerido
        </Alert>
        <Button :loading="loading" @click="handleSend" class="mt-6" block yellow>
          Verificar
        </Button>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'
import axios from '~/plugins/axios'
@Component
export default class transfer extends Vue {
  send: boolean = false
  loading: boolean = false
  transferred: boolean = false

  ekambiaAccounts: any = []
  account: any = null

  form: any = {
    file: null,
    ref: null,
  }

  data: any = null

  getOperation() {
    axios.get(`/operation-show/${this.$route.query.id}`).then(({data}: any) => {
      console.log(data)
      this.data = data.info
    })
  }

  handleChange( val, text, {obj}) {
    this.account = obj
  }

  handleSend() {
    this.send = true
    this.$router.push({
        path: 'step3',
        query: {
          ...this.$route.query
        }
      })
    if (!this.form.file && !this.form.ref) {
      return
    }
    this.loading = true
    var formData = new FormData()
    if (this.form.file) {
      formData.append('voucher', this.form.file)
    }
    if (this.form.ref) {
      formData.append('num_reference', this.form.ref)
    }
    axios.post(`/operation-voucherupdate/${this.$route.query.id}`, formData, {
      headers: {
        'Content-Type': 'multipart/form-data'
      }
    }).then(() => {
      this.loading = false
      this.$router.push({
        path: 'step3',
        query: {
          ...this.$route.query
        }
      })
    }).catch((err) => {
      this.loading = false
      this.$notification({
        title: 'Oops! Algo salió mal',
        text: err.response.data.message.toString()
      })
    })
  }

  getAccounts() {

    axios.get(`/accountekambia`).then(({data}) => {
      this.ekambiaAccounts = data.info
    })
  }

  mounted() {
    this.getAccounts()
    this.getOperation()
  }
}
</script>
<style lang="sass" scoped>
.card-texts
  background: -color('gray-2')
  border-radius: 30px
  width: 100%
  margin-top: 20px
  .texts
    background: #fff
    border-radius: 30px
    display: flex
    align-items: center
    justify-content: center
    .text
      width: 50%
      text-align: center
      padding: 15px 10px
  .textw
    width: 100%
    text-align: center
    padding: 10px
  span
    opacity: .5
    font-size: .8rem
    font-weight: bold
  p
    font-size: .9rem
    font-weight: bold
.con-data
  ul
    width: 100%
    margin-bottom: 20px
    padding: 10px
    li
      display: flex
      align-items: center
      justify-content: flex-start
      width: 100%
      padding: 10px 0px
      p
        display: block
        margin-left: 10px
.transfer
  width: 100%
  padding: 15px
  h2
    font-size: 1.1rem
    text-align: center
    padding: 10px 0px
    margin-top: 10px
</style>
