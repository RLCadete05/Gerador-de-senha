<template>
  <v-container class="fill-height main-container" fluid="false">
    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="6">
        <v-card class="pa-4 password-card">
          <div class="logo-container mb-4">
            <v-icon size="64" color="#843d6c" class="logo-icon">mdi-shield-lock</v-icon>
          </div>
          
          <v-card-title class="text-h4 text-center mb-4 gradient-text">
            Gerador de Senhas
          </v-card-title>

          <v-card-text>
            <p class="mb-2 security-text">
              A maneira mais segura e eficiente de gerar senhas fortes. 
              Suas senhas são geradas localmente e <strong>nunca são armazenadas</strong>.
            </p>            
            <p class="font-weight-bold mb-6 security-text">
              Use senhas fortes, mas salve-as em um local seguro!
            </p>

            <v-text-field
              v-model="password"
              label="Sua senha"
              readonly
              variant="outlined"
              class="mb-4 custom-field"
            >
              <template v-slot:append>
                <v-btn
                  icon="mdi-content-copy"
                  @click="copyPassword"
                  :disabled="!password"
                  class="copy-btn"
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
              class="mb-4 custom-slider"
            />

            <v-checkbox
              v-model="options.uppercase"
              label="Incluir letras maiúsculas"
              class="custom-checkbox"
            />
            <v-checkbox
              v-model="options.numbers"
              label="Incluir números"
              class="custom-checkbox"
            />
            <v-checkbox
              v-model="options.symbols"
              label="Incluir caracteres especiais"
              class="custom-checkbox"
            />
          </v-card-text>

          <v-card-actions>
            <v-btn
              block
              class="generate-btn"
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
      location="top"
    >
      {{ snackbar.text }}
    </v-snackbar>

    <Footer />
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
  
  // Mostrar mensagem de sucesso
  snackbar.value = {
    show: true,
    text: 'Senha gerada com sucesso!',
    color: 'success'
  }
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
.main-container {
  background: linear-gradient(135deg, #e1e1e1 0%, #ffffff 100%);
  min-height: 100vh;
}

.password-card {
  border-radius: 16px;
  background: rgba(255, 255, 255, 0.95);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.logo-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.logo-icon {
  background: linear-gradient(45deg, #843d6c, #c8537c);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.gradient-text {
  background: linear-gradient(45deg, #843d6c, #c8537c);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: bold;
}

.security-text {
  color: #726d86;
  font-size: 0.9rem;
  line-height: 1.5;
}

.custom-field {
  border-radius: 8px;
}

.custom-field :deep(.v-field__outline) {
  border-color: #e9aec8;
}

.custom-slider :deep(.v-slider-thumb__label) {
  background: #843d6c;
}

.custom-slider :deep(.v-slider-track__fill) {
  background: #c8537c;
}

.custom-checkbox :deep(.v-selection-control) {
  color: #843d6c;
}

.generate-btn {
  background: linear-gradient(45deg, #843d6c, #c8537c) !important;
  color: white !important;
  font-weight: bold;
  text-transform: none;
  letter-spacing: 0.5px;
  height: 48px;
  border-radius: 8px;
}

.copy-btn {
  color: #843d6c;
}
</style>
