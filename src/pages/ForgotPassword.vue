<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handleForgotPass">
      <p class="col-12 text-h5 text-center">Recuperação de Senha</p>
      <div class="col-md-4 col-sm-6 col-xs-10 q-gutter-md">
        <q-input
          label="Email"
          v-model="email"
          lazy-rules
          :rules="[val => val && val.length > 0 || 'Por favor, informe o e-mail']"
          type="email"
        />

        <div class="full-width q-pt-md q-gutter-y-sm">
          <q-btn
            label="Enviar e-mail de recuperação"
            color="primary"
            class = "full-width"
            outline
            rounded
            type="submit"
            />

            <q-btn
            label="Voltar"
            color="primary"
            class = "full-width"
            flat
            rounded
            :to="{ name: 'login' }"
            />
          </div>
      </div>
    </q-form>
  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue'
import useAuthUser from 'src/composables/useAuthUser'
import useNotify from 'src/composables/useNotify'

export default defineComponent({
  setup () {
    const { sendPasswordResetEmail } = useAuthUser()

    const email = ref('')

    const { notifyError, notifySuccess } = useNotify()

    const handleForgotPass = async () => {
      try {
        await sendPasswordResetEmail(email.value)
        notifySuccess(`E-mail de recuperação enviado para: ${email.value}`)
      } catch (error) {
        notifyError(error.message)
      }
    }

    return {
      email,
      handleForgotPass
    }
  }
})
</script>
