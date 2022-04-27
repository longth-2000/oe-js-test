<template>
  <div class="detail-store-container container-fluid">
    <div class="store-title">
      <h1>{{ title }}</h1>
    </div>
    <div class="store-filter">
      <div class="button">
        <button @click="showFilter = true">Filter</button>
      </div>
      <div>
        <select
          class="responsiveFilter"
          name="filter"
          id="filter"
          @change="filterResponsive()"
        >
          <option v-for="(filter, index) in toppingFilter" :key="index">
            {{ filter }}
          </option>
        </select>
      </div>
      <div class="select">
        <span>Sort by </span>
        <select name="sort" id="sort" @change="sortMenu()">
          <option value="name/1">Name (ASC)</option>
          <option value="name/-1">Name (DSC)</option>
          <option value="price/1">Price (ASC)</option>
          <option value="price/-1">Price (DSC)</option>
        </select>
      </div>
    </div>
    <div class="toppings-filter container-fluid" v-if="showFilter">
      <div
        class="title"
        style="text-align: left; margin: 0 0 20px 25px; font-weight: bold"
      >
        Toppings:
      </div>
      <div class="row">
        <div
          class="topping-option col-md-3"
          v-for="(filter, index) in toppingFilter"
          :key="index"
        >
          <input
            type="radio"
            :value="filter"
            name="filter-topping"
            @change="filterToppings(filter)"
          />
          {{ filter }}
          <!-- <label :for="filter">{{ filter }}</label><br> -->
        </div>
      </div>
    </div>
    <div class="store-menu row">
      <div
        v-for="(card, index) in menu"
        :key="index"
        class="col-6 col-sm-4 col-md-3 menu-items"
      >
        <store-card :productID="card.product" :index="index"></store-card>
      </div>
    </div>
  </div>
</template>
<script>
import StoreCard from "../components/StoreCard.vue";
import products from "../data/products.json";
import storeProducts from "../data/storeProducts.json";

export default {
  components: {
    StoreCard,
  },
  props: {
    title: {
      type: String,
    },
    storeID: {
      type: Number,
    },
  },
  created() {
    this.getMenu(this.storeID);
  },
  data() {
    return {
      productLists: products.products,
      menu: [],
      menuOriginal: [],
      storeProducts: storeProducts.shopProducts,
      showFilter: false,
    };
  },
  watch: {
    storeID(newVal) {
      this.getMenu(newVal);
      this.showFilter = false;
    },
  },
  computed: {
    toppingFilter() {
      const productFilter = this.productLists
        .reduce(
          (previousValue, currentValue) => [
            ...previousValue,
            ...currentValue.toppings.split(","),
          ],
          []
        )
        .filter(
          (value, index, self) =>
            index ===
            self.findIndex((item) => item.toLowerCase() === value.toLowerCase())
        );
      return productFilter;
    },
  },
  methods: {
    getMenu(storeID) {
      this.menu = this.storeProducts.filter((item) => item.shop === storeID);
      this.menuOriginal = [...this.menu];
    },
    filterToppings(value) {
      this.filterOption(value);
    },
    filterResponsive() {
      let value = document.querySelector("#filter").value;
      this.filterOption(value);
    },
    filterOption(value) {
      let filters = this.productLists.filter((element) => {
        let regexPattern = `[A-Za-z]${value.slice(1)}`;

        let regexTopping = new RegExp(regexPattern, "g");
        console.log(regexTopping);
        return regexTopping.test(element.toppings);
      });
      this.menu = this.menuOriginal.filter((element) => {
        console.log(element);
        console.log(filters.some((item) => element.product === item.id));
        return filters.some((item) => element.product === item.id);
      });
      console.log(this.menu);
    },
    sortMenu() {
      const sortValue = document.querySelector("#sort").value;
      const [name, option] = sortValue.split("/");
      this.handleSort(name, option);
    },
    getProductByID(id) {
      return this.productLists.filter((element) => element.id === id)[0];
    },
    handleSort(name, option) {
      this.menu.sort((first, second) => {
        const firstPro = this.getProductByID(first.product);
        const secondPro = this.getProductByID(second.product);
        return name === "name"
          ? firstPro[name].localeCompare(secondPro[name])
          : firstPro[name] - secondPro[name];
      });
      if (option === "-1") this.menu = this.menu.reverse();
    },
  },
};
</script>
<style>
.detail-store-container {
  width: 96%;
  margin-left: 20px;
}
.store-title {
  margin: 20px 0;
}
.store-filter {
  margin-top: 50px;
  height: 70px;
}
.store-filter button {
  padding: 10px 50px;
  float: left;
  border: none;
  border-radius: 4px;
  background: #06035d;
  color: white;
}
.store-filter .select {
  float: right;
}
.store-filter .select {
  font-weight: bold;
}
.store-filter select {
  padding: 10px;
  border-radius: 4px;
  background: #cdd9cd;
  border: 1px solid black;
}
.store-menu {
  width: 100%;
  height: 100%;
}
.menu-items {
  margin: 10px 50px 10px 0;
}
.toppings-filter {
  width: 100%;
  height: 100px;
  background: white;
  padding: 10px;
  border-radius: 4px;
  margin: 10px 0;
}
.responsiveFilter {
  display: none;
}
@media only screen and (max-width: 768px) {
  .store-title {
    padding: 20px 0 0 0;
  }
}
@media only screen and (max-width: 692px) {
  .store-filter button {
    display: none;
  }
  .responsiveFilter {
    display: block;
    background: #06035d !important;
    color: white !important;
  }
  .store-filter .select {
    width: 100%;
    margin: 20px 0;
  }
  .store-filter select {
    width: 100%;
  }
  .row {
    width: 50%;
    margin: 50px auto;
  }
}
@media only screen and (max-width: 376px) {
  .detail-store-container {
    margin-left: 0;
  }
  .row {
    width: 65%;
  }
  .store-list-items {
    padding: 0;
    font-size: 14px;
  }
}
</style>
