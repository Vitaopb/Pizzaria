<template>
  <v-dialog v-model="isOpen" max-width="600px">
    <v-card>
      <v-card-title>Cardápio</v-card-title>
      <v-card-text>
        <v-carousel>
          <v-carousel-item v-for="(image, index) in images" :key="index">
            <v-img contain style="height: 100% !important;" :src="image"></v-img>
          </v-carousel-item>
        </v-carousel>
      </v-card-text>
      <v-card-actions>
        <v-btn text @click="closeModal">Fechar</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: ['images'],
  data() {
    return {
      isOpen: false,
    };
  },
  methods: {
    closeModal() {
      this.isOpen = false;
    },
  },
};
</script>
