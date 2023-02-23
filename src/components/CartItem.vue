<template>
  <tr >
    <td class="is-flex is-align-items-center">
       <img v-bind:src="item.product.get_thumbnail" width="100">
      <span><router-link :to="item.product.get_absolute_url">{{ item.product.name }}</router-link></span>
    </td>
    <td class="pt-6">${{ item.product.price }}</td>

    <td class="pt-6">
      <a @click="decrementQuantity(item)">-</a>
      <span class="px-5">{{ item.quantity }}</span>
      <a @click="incrementQuantity(item)">+</a>
    </td>
   <td class="pt-6">${{ getItemTotal(item).toFixed(2) }}</td>
   <td class="pt-6">
      <button class="delete" @click="removeFromCart(item)"></button>
    </td>
  </tr>
</template>

<script>
export default {
  name: 'CartItem',
  props: {
    initialItem: Object
  },
  data() {
    return {
      item: this.initialItem
    }
  },
  methods: {
    getItemTotal(item) {
      return item.quantity * item.product.price
    },
    decrementQuantity(item) {
      item.quantity -= 1

      if (item.quantity === 0) {
        this.$emit('removeFromCart', item)
      }

      this.updateCart()
    },
    incrementQuantity(item) {
      item.quantity += 1

      this.updateCart()
    },
    updateCart() {
      localStorage.setItem('cart', JSON.stringify(this.$store.state.cart))
    },
    removeFromCart(item) {
      this.$emit('removeFromCart', item)

      this.updateCart()
    },
  },
}
</script>