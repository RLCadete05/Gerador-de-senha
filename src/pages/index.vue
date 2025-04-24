<template>
  <v-container class="fill-height">
    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="6">
        <v-card class="pa-4">
          <v-card-title class="text-h4 text-center mb-4">
            Gerador de Senhas
          </v-card-title>

          <v-card-text>
            <v-text-field
              v-model="password"
              label="Sua senha"
              readonly
              variant="outlined"
              class="mb-4"
            >
              <template v-slot:append>
                <v-btn
                  icon="mdi-content-copy"
                  @click="copyPassword"
                  :disabled="!password"
                />
              </template>
            </v-text-field>

            <v-slider
              v-model="length"
              label="Tamanho da senha"
              min="4"
              max="32"
              step="1"
              thumb-label
              class="mb-4"
            />

            <v-checkbox
              v-model="options.uppercase"
              label="Incluir letras maiúsculas"
              class="mb-2"
            />
            <v-checkbox
              v-model="options.numbers"
              label="Incluir números"
              class="mb-2"
            />
            <v-checkbox
              v-model="options.symbols"
              label="Incluir caracteres especiais"
              class="mb-2"
            />
          </v-card-text>

          <v-card-actions>
            <v-btn
              block
              color="deep-purple-darken-1"
              @click="generatePassword"
              :disabled="!isValidOptions"
            >
              Gerar Nova Senha
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>

    <v-snackbar
      v-model="snackbar.show"
      :color="snackbar.color"
      :timeout="2000"
    >
      {{ snackbar.text }}
    </v-snackbar>
  </v-container>
</template>

<script setup>
import { ref, computed } from 'vue'

const password = ref('')
const length = ref(12)
const options = ref({
  uppercase: true,
  numbers: true,
  symbols: true
})

const snackbar = ref({
  show: false,
  text: '',
  color: 'success'
})

const isValidOptions = computed(() => {
  return options.value.uppercase || options.value.numbers || options.value.symbols
})

const generatePassword = () => {
  const lowercase = 'abcdefghijklmnopqrstuvwxyz'
  const uppercase = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
  const numbers = '0123456789'
  const symbols = '!@#$%^&*()_+-=[]{}|;:,.<>?'

  let chars = lowercase
  if (options.value.uppercase) chars += uppercase
  if (options.value.numbers) chars += numbers
  if (options.value.symbols) chars += symbols

  let result = ''
  for (let i = 0; i < length.value; i++) {
    result += chars.charAt(Math.floor(Math.random() * chars.length))
  }

  password.value = result
}

const copyPassword = async () => {
  try {
    await navigator.clipboard.writeText(password.value)
    snackbar.value = {
      show: true,
      text: 'Senha copiada com sucesso!',
      color: 'success'
    }
  } catch (err) {
    snackbar.value = {
      show: true,
      text: 'Erro ao copiar senha',
      color: 'error'
    }
  }
}

// Gerar uma senha inicial
generatePassword()
</script>

<style scoped>
.v-card {
  border-radius: 12px;
}
</style>
