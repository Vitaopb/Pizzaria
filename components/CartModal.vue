<template>
  <v-dialog v-model="isOpen" max-width="500px">
    <v-card>
      <v-card-title>Resumo do Pedido</v-card-title>
      <v-card-text>
        <div v-if="cart.length">
          <v-list>
            <v-list-item v-for="(item, index) in cart" :key="index">
              <v-list-item-content>
                <v-list-item-title>{{ item.quantity }}x {{ item.name }} ({{ item.size }})</v-list-item-title>
                <v-list-item-subtitle>R$ {{ item.price }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
          <v-form ref="form" v-model="valid" lazy-validation>
            <v-text-field label="Nome" v-model="formData.name" :rules="[rules.required]" required></v-text-field>
            <v-text-field label="Telefone" v-model="formData.phone" :rules="[rules.required]" required></v-text-field>
            <v-text-field label="Nome da Rua" v-model="formData.street" :rules="[rules.required]" required></v-text-field>
            <v-text-field label="Número da Casa" v-model="formData.number" :rules="[rules.required]" required></v-text-field>
            <v-text-field label="Bairro" v-model="formData.neighborhood" :rules="[rules.required]" required></v-text-field>
            <v-text-field label="Ponto de Referência" v-model="formData.reference"></v-text-field>
          </v-form>
        </div>
        <div v-else>
          <p>O carrinho está vazio.</p>
        </div>
      </v-card-text>
      <v-card-actions>
        <v-btn :disabled="!valid" color="orange" @click="placeOrder">Fazer Pedido</v-btn>
        <v-btn text @click="closeModal">Fechar</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: ['cart'],
  data() {
    return {
      isOpen: false,
      valid: false,
      formData: {
        name: '',
        phone: '',
        street: '',
        number: '',
        neighborhood: '',
        reference: '',
      },
      rules: {
        required: value => !!value || 'Este campo é obrigatório.',
      },
    };
  },
  methods: {
    closeModal() {
      this.isOpen = false;
    },
    placeOrder() {
      const now = new Date();
      const formattedDate = `${now.toLocaleDateString()} ${now.toLocaleTimeString()}`;
      const message = `*NOVO PEDIDO* - ${formattedDate}

*Dados para Entrega:*
- *Nome:* ${this.formData.name}
- *Telefone:* ${this.formData.phone}
- *Endereço:* ${this.formData.street}, ${this.formData.number}, ${this.formData.neighborhood}
- *Ponto de Referência:* ${this.formData.reference || 'N/A'}

*Dados do Pedido:*
${
this.cart
.map(
item => `- ${item.quantity}x ${item.name} (${item.size}) - R$ ${item.price}`
)
.join('')
}

_____________________________________________________________________

*Total da Compra:* R$ ${this.cart.reduce((total, item) => total + item.price * item.quantity, 0).toFixed(2)};
        `
      window.open(`https://wa.me/5581993341500?text=${encodeURIComponent(message)}`);
    },
  },
};
</script>
