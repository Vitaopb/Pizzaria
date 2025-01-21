<template>
  <v-card class="mb-4">
    <v-img :src="item.image" aspect-ratio="1.5"></v-img>
    <v-card-title>{{ item.title }}</v-card-title>
    <v-card-subtitle>{{ item.description }}</v-card-subtitle>
    <v-row align="center" justify="space-between" class="mt-3 pa-3" dense>
      <v-col cols="12">
        <v-select
          :items="item.sizes"
          label="Tamanho"
          v-model="selectedSize"
          outlined
          dense
          class="primary-input"
        ></v-select>
      </v-col>
      <v-col cols="12" class="d-flex justify-center align-center" style="gap: 10px;">
        <v-icon size="32" @click="decreaseQuantity">mdi-minus-circle</v-icon>
        <div style="font-size: 30px !important; font-weight: bold;">{{ quantity }}</div>
        <v-icon size="32" @click="increaseQuantity">mdi-plus-circle</v-icon>
      </v-col>
    </v-row>
    <v-card-actions>
      <v-btn
        class="primary-btn"
        style="width: 100% !important; color: black !important"
        color="orange"
        @click="addToCart"
        >Adicionar ao Carrinho</v-btn
      >
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: ["item"],
  data() {
    return {
      selectedSize: null,
      quantity: 0,
    };
  },
  methods: {
    decreaseQuantity() {
      if (this.quantity > 0) this.quantity--;
    },
    increaseQuantity() {
      this.quantity++;
    },
    addToCart() {
      if (this.quantity > 0 && this.selectedSize) {
        this.$emit("add-to-cart", {
          name: this.item.title,
          size: this.selectedSize,
          quantity: this.quantity,
          price: this.item.sizes.find(
            (size) => size.value === this.selectedSize
          ).price,
        });
      } else {
        this.$emit("error", "Selecione um tamanho e uma quantidade válida.");
      }
    },
  },
};
</script>
