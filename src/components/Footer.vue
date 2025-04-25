<template>
  <v-footer app class="d-flex justify-center align-center flex-column" style="position: absolute; background: linear-gradient(135deg, #0d1039 0%, #843d6c 100%);">
    <span class="text-white text-center mt-3">
      &copy; {{ new Date().getFullYear() }} Raquel Cadete. Todos os direitos reservados.
    </span>
    <div>
      <!-- <span class="text-white text-h6 me-4">Onde me encontrar:</span> -->
      <v-btn
        icon="mdi-linkedin"
        color="white"
        variant="text"
        href="https://www.linkedin.com/in/raquel-c-869356b5/"
        target="_blank"
        class="mx-2"
      />
      <v-btn
        icon="mdi-github"
        color="white"
        variant="text"
        href="https://github.com/RLCadete05"
        target="_blank"
        class="mx-2"
      />
    </div>
  </v-footer>
</template>

<script setup>

</script>