<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handleRegister">
      <p class="col-12 text-h5 text-center">Cadastro</p>
      <div class="col-md-4 col-sm-6 col-xs-10 q-gutter-md">
        <q-input
          label="Nome"
          v-model="form.name"
          lazy-rules
          :rules="[val => val && val.length > 0 || 'Por favor, informe o nome']"
        />

        <q-input
          label="Email"
          v-model="form.email"
          lazy-rules
          :rules="[val => val && val.length > 0 || 'Por favor, informe o e-mail']"
          type="email"
        />

        <q-input
          label="Password"
          v-model="form.password"
          lazy-rules
          :rules="[val => val && val.length >= 6 || 'Por favor, informe uma senha válida (Mínimo 6 caracteres)']"
        />

        <div class="full-width q-pt-md q-gutter-y-sm">
          <q-btn
            label="Registro"
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
import { useRouter } from 'vue-router'

export default defineComponent({
  name: 'PageRegister',

  setup () {
    const router = useRouter()
    const { register } = useAuthUser()

    const { notifyError, notifySuccess } = useNotify()

    const form = ref({
      name: '',
      email: '',
      password: ''
    })

    const handleRegister = async () => {
      try {
        await register(form.value)
        notifySuccess()
        router.push({
          name: 'email-confirmation',
          query: { email: form.value.email }
        })
      } catch (error) {
        notifyError(error)
      }
    }

    return {
      form,
      handleRegister
    }
  }
})
</script>
