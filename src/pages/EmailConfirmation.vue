<template>
  <q-page padding>
    <p class="text-body1">Obrigado por se cadastrar!</p>
    <p class="text-body1">Por favor confirme seu e-mail para finalizar o cadastro: <strong> {{ $route.query.email }} </strong></p>
    <div class="row justify-centar">
      <div class="col-md-4 col-sm-6 col-xs-10 q-gutter-md">
        <q-btn
          label="Voltar ao Login"
          color="primary"
          class = "full-width"
          rounded
          :to="{ name: 'login' }"
        />
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'PageEmailConfirmation'
})
</script>
