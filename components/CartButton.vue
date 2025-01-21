<template>
  <v-btn fab color="orange" class="cart-button" @click="$emit('open-cart')">
    <v-icon>mdi-cart</v-icon>
    <span v-if="cartCount" class="cart-count">{{ cartCount }}</span>
  </v-btn>
</template>

<script>
export default {
  props: ['cartCount'],
};
</script>

<style>
.cart-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
.cart-count {
  position: absolute;
  top: -10px;
  right: -10px;
  background: red;
  color: white;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
}
</style>
