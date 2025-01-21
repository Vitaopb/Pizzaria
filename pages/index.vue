<template>
  <v-container>
    <v-row justify="center">
      <v-btn color="orange" @click="openMenu">Ver Cardápios</v-btn>
    </v-row>
    <v-row>
      <v-col cols="12" md="4">
        <h2>Pizzas</h2>
        <Card
          v-for="(item, index) in pizzas"
          :key="index"
          :item="item"
          @add-to-cart="addToCart"
          @error="showError"
        />
      </v-col>
      <v-col cols="12" md="4">
        <h2>Pizzas Doces</h2>
        <Card
          v-for="(item, index) in sweetPizzas"
          :key="index"
          :item="item"
          @add-to-cart="addToCart"
          @error="showError"
        />
      </v-col>
      <v-col cols="12" md="4">
        <h2>Açaí</h2>
        <Card
          v-for="(item, index) in acai"
          :key="index"
          :item="item"
          @add-to-cart="addToCart"
          @error="showError"
        />
      </v-col>
    </v-row>
    <CartButton @open-cart="openCart" />
    <CartModal :cart="cart" ref="cartModal" />
    <MenuModal :images="menuImages" ref="menuModal" />
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      pizzas: [
        {
          title: 'Calabresa',
          description: 'Calabresa, cebola, mussarela e orégano',
          image: 'download.jpg',
          sizes: [
            { text: 'Pequena - R$ 23,00', value: 'P', price: 23 },
            { text: 'Média - R$ 34,00', value: 'M', price: 34 },
            { text: 'Grande - R$ 42,00', value: 'G', price: 42 },
          ],
        },
      ],
      sweetPizzas: [
        {
          title: 'Romeu e Julieta',
          description: 'Goiabada e mussarela',
          image: 'download (1).jpg',
          sizes: [
            { text: 'Pequena - R$ 23,00', value: 'P', price: 23 },
            { text: 'Média - R$ 34,00', value: 'M', price: 34 },
            { text: 'Grande - R$ 42,00', value: 'G', price: 42 },
          ],
        },
      ],
      acai: [
        {
          title: 'Açaí 300ml',
          description: 'Açaí com leite condensado, granola e banana',
          image: 'acai.jpg',
          sizes: [
            { text: '300ml - R$ 7,00', value: '300ml', price: 7 },
            { text: '350ml - R$ 10,00', value: '350ml', price: 10 },
          ],
        },
      ],
      cart: [],
      menuImages: [
        '/cardapio-pizzaria.jpg',
        '/cardapio-pizza-2.jpg',
      ],
    };
  },
  methods: {
    addToCart(item) {
      this.cart.push(item);
    },
    openCart() {
      this.$refs.cartModal.isOpen = true;
    },
    openMenu() {
      this.$refs.menuModal.isOpen = true;
    },
    showError(message) {
      alert(message);
    },
  },
};
</script>
