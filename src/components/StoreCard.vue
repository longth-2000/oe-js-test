<template>
  <div class="store-card">
    <div class="store-card-content">
      <div class="title">
        <p>MT-0{{ index + 1 }}</p>
        <p class="bold">{{ product.name }}</p>
      </div>
      <hr />
      <div class="toppings">
        <p class="bold">Toppings:</p>
        <p>{{ product.toppings }}</p>
      </div>
      <div class="price bold">{{ product.price }}$</div>
    </div>
  </div>
</template>
<script>
import products from "../data/products.json";
export default {
  props: {
    productID: {
      type: Number,
    },
    index: {
      type: Number,
    },
  },
  data() {
    return {
      product: {},
      products: products.products,
    };
  },
  watch: {
    productID(newVal) {
      this.getDetailProduct(newVal);
    },
  },
  created() {
    this.getDetailProduct(this.productID);
  },
  methods: {
    getDetailProduct(productID) {
      this.product = this.products.filter((item) => item.id === productID)[0];
    },
  },
};
</script>
<style scoped>
.store-card {
  height: 100%;
  width: 190px;
  background: white;
  border-radius: 4px;
  padding: 5px 20px;
  color: #020252;
  position: relative;
}
.store-card-content {
}
.store-card-content p {
  text-align: left;
}
.store-card-content .bold {
  font-weight: bold;
}
.store-card-content .price {
  position: absolute;
  right: 10px;
  bottom: 10px;
}
</style>
