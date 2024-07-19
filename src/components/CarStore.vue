<template>
  <div class="container">
    <h1>Carrito de compras</h1>

    <div class="split-container">
      <!-- Productos disponibles -->
      <div class="split-item">
        <h2>Productos disponibles</h2>
        <ul>
          <li v-for="product in products" :key="product.id" class="product-item">
            <img :src="require(`@/assets/img/product-${product.id}.jpg`)" alt="Imagen del producto" /><br>
            <div class="product-info">
              <span>{{ product.name }} - Precio $: {{ product.price }}</span>
              <span v-if="product.stock > 0">Disponibles: {{ product.stock }}</span>
              <button @click="addToCart(product)" class="add-to-cart-btn" :disabled="product.stock === 0">
                <i class="fa fa-shopping-cart" aria-hidden="true"></i>
                Agregar al carrito
              </button>
            </div>
          </li>
        </ul>
      </div>

      <!-- Productos en el carrito -->
      <div class="split-item">
        <h2>Productos en el carrito</h2>
        <ul>
          <li v-for="item in cart" :key="item.product.id" class="cart-item">
            <img :src="require(`@/assets/img/product-${item.product.id}.jpg`)" alt="Imagen del producto" />
            <div class="cart-info">
              <span>{{ item.product.name }} - Cantidad: {{ item.quantity }}</span>
              <button @click="removeFromCart(item)" class="remove-from-cart-btn">
                <i class="fa fa-trash" aria-hidden="true"></i>
                Remover del carrito
              </button>
            </div>
          </li>
        </ul>

        
      </div>
    </div>
        <!-- Total a pagar -->
        <p class="total-label">Total a pagar: {{ total }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        { id: 1, name: 'Audífono', price: 30000, stock: 3 },
        { id: 2, name: 'Mouse', price: 20000, stock: 5 },
        { id: 3, name: 'Teclado', price: 15000, stock: 10 },
        { id: 4, name: 'Gabinete', price: 35000, stock: 4 },
        { id: 5, name: 'Pantalla', price: 175000, stock: 3 },
        { id: 6, name: 'Silla', price: 150000, stock: 2 }
      ],
      cart: []
    };
  },
  computed: {
    total() {
      return this.cart.reduce((total, item) => total + (item.product.price * item.quantity), 0);
    }
  },

methods: {
    addToCart(product) {
      if (product.stock > 0) {
        let cartItem = this.cart.find(item => item.product.id === product.id);
        if (cartItem) {
          cartItem.quantity++;
        } else {
          this.cart.push({ product: product, quantity: 1 });
        }
        product.stock--;
        if (product.stock === 0) {
          alert("No hay más unidades en stock!");
        }
      } else {
        alert("No hay suficiente stock para este producto!");
      }
    },

    removeFromCart(item) {
      let index = this.cart.indexOf(item);
      if (index !== -1) {
        item.product.stock += item.quantity;
        if (item.quantity > 1) {
          item.quantity--;
        } else {
          this.cart.splice(index, 1);
        }
      }
    }
  }
};
</script>

<style scoped>
.container {
  width: 80%;
  margin: 0 auto;
  text-align: center;
}

h1 {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}

.split-container {
  display: flex;
  justify-content: space-between;
}

.split-item {
  flex: 1;
  margin: 0 10px;
}

ul {
  list-style: none;
  padding: 0;
}

.product-item,
.cart-item {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.product-info,
.cart-info {
  flex: 1;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

img {
  width: 50px;
  height: 50px;
  margin-right: 10px;
}

.add-to-cart-btn,
.remove-from-cart-btn {
    background-color: #76448A;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    display: grid;
    margin-top: 10px;
    width: 50%;
}

.add-to-cart-btn i,
.remove-from-cart-btn i {
  margin-right: 10px;
}

.total-label {
  font-weight: bold;
  font-size: 20px;
  margin-top: 20px;
}
</style>