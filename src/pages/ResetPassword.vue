<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handlePassReset">
      <p class="col-12 text-h5 text-center">Recuperação de Senha</p>
      <div class="col-md-4 col-sm-6 col-xs-10 q-gutter-md">
        <q-input
          label="Nova senha"
          v-model="password"
          lazy-rules
          :rules="[val => val && val.length >= 6 || 'Por favor, informe uma senha válida (Mínimo 6 caracteres)']"
        />

        <div class="full-width q-pt-md q-gutter-y-sm">
          <q-btn
            label="Salvar nova senha"
            color="primary"
            class = "full-width"
            outline
            rounded
            type="submit"
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
import { useRouter, useRoute } from 'vue-router'

export default defineComponent({
  name: 'PageResetPassword',

  setup () {
    const { resetPassword } = useAuthUser()

    // Rota a ser redirecionada
    const router = useRouter()
    // Rota atual
    const route = useRoute()
    const token = route.query.token

    const password = ref('')

    const { notifyError, notifySuccess } = useNotify()

    const handlePassReset = async () => {
      try {
        await resetPassword(token, password.value)
        notifySuccess('Senha alterada com sucesso!')
        router.push({ name: 'login' })
      } catch (error) {
        notifyError(error.message)
      }
    }

    return {
      password,
      handlePassReset
    }
  }
})
</script>
