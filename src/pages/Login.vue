<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handleLogin">
      <p class="col-12 text-h5 text-center">Login</p>
      <div class="col-md-4 col-sm-6 col-xs-10 q-gutter-md">
        <q-input
          label="Email"
          v-model="form.email"
          lazy-rules
          :rules="[val => val && val.length > 0 || 'Por favor, informe o e-mail']"
          type="email"
        />

        <q-input
          label="Senha"
          v-model="form.password"
          lazy-rules
          :rules="[val => val && val.length > 0 || 'Por favor, informe uma senha válida']"
          type="password"
        />

        <div class="full-width q-pt-md">
          <q-btn
            label="Login"
            color="primary"
            class = "full-width"
            outline
            rounded
            type="submit"
            />
          </div>
          <div class="full-width q-gutter-y-md">
          <q-btn
            label="Registrar"
            color="primary"
            class = "full-width"
            flat
            to="/register"
            size="sm"
            />

            <q-btn
            label="Esqueceu a senha?"
            color="primary"
            class = "full-width"
            flat
            :to=" { name:'forgot-password' } "
            size="sm"
            />
          </div>
      </div>
    </q-form>
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'
import useAuthUser from 'src/composables/useAuthUser'
import useNotify from 'src/composables/useNotify'
import { useRouter } from 'vue-router'

export default defineComponent({
  name: 'PageLogin',

  setup () {
    const router = useRouter()

    const { login, isLoggedIn } = useAuthUser()

    const form = ref({
      email: '',
      password: ''
    })

    onMounted(() => {
      if (isLoggedIn) {
        router.push({ name: 'me' })
      }
    })

    const { notifyError, notifySuccess } = useNotify()

    const handleLogin = async () => {
      try {
        await login(form.value)
        notifySuccess('Login realizado com sucesso!')
        router.push({ name: 'me' })
      } catch (error) {
        notifyError(error.message)
      }
    }
    return {
      form,
      handleLogin
    }
  }
})
</script>
