<template>
  <div class="con-create">
    <div class="con-form">
      <h2 class="mb-6">
        Crear tu cuenta
      </h2>

      <input-file
        ref="front"
        v-model="form.cedulaFront"
        :danger="!form.cedulaFront && send"
        :disabled="loading"
      >
        Foto de tu DNI (frontal)
      </input-file>
      <Alert :open="!form.cedulaFront && send">
        Este campo es requerido
      </Alert>
      <input-file
        v-model="form.cedulaPost"
        :danger="!form.cedulaPost && send"
        :disabled="loading"
      >
        Foto de tu DNI (Posterior)
      </input-file>
      <Alert :open="!form.cedulaPost && send">
        Este campo es requerido
      </Alert>
      <input-file
        v-model="form.selfie"
        :danger="!form.selfie && send"
        user
        :disabled="loading"
      >
        Tómate un selfie
      </input-file>
      <Alert :open="!form.selfie && send">
        Este campo es requerido
      </Alert>
    </div>
      <Button :loading="loading" @click="handleSend" class="mb-6 mt-6" block yellow>
        Siguiente
      </Button>

  </div>
</template>
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import axios from '~/plugins/axios'
@Component
export default class createAccount extends Vue {
  send: boolean = false
  loading: boolean = false
  form: any = {
    cedulaFront: '',
    cedulaPost: '',
    selfie: ''
  }

  handleSend() {
    this.send = true
    if (!this.form.cedulaFront || !this.form.cedulaPost || !this.form.selfie) {
      return
    }
    this.loading = true
    var formData = new FormData()
    formData.append("cedulaFront", this.form.cedulaFront)
    formData.append("cedulaPost", this.form.cedulaPost)
    formData.append("selfie", this.form.selfie)
    axios.post('/update-userimages', formData, {
      headers: {
        'Content-Type': 'multipart/form-data'
      }
    }).then((res) => {
      this.loading = false
      this.$router.push('/createAccount/step4')
    }).catch((err) => {
      this.loading = false
      this.$notification({
        title: 'Oops! Algo salió mal',
        text: err.response.data.message.toString()
      })
    })
  }

  mounted() {
    this.$bounceClose()

    if (this.$route.query.check) {
      setTimeout(() => {
        this.$notification({
          title: 'Bienvenido de nuevo',
          text: 'Para poder continuar necesitamos terminar de verificar tus datos'
        })
      }, 600);
    }
  }
}
</script>
<style lang="sass" scoped>
// responsive
.con-select-input
  display: flex
  align-items: center
  justify-content: space-between
  width: 100%
.con-create
  width: 100%
  overflow: hidden
  flex: 1
  display: flex
  align-items: center
  justify-content: center
  flex-direction: column
  h2
    font-weight: 500
    text-align: center
    width: 100%
.con-form
  display: flex
  align-items: center
  justify-content: flex-start
  flex-direction: column
  flex: 1
  width: 100%
  max-width: 400px
  // overflow: auto
  // max-height: calc(100vh - 110px)
  // padding-bottom: 40px


@media (min-width: 812px)
  .con-create
    .button
      max-width: 400px
</style>
