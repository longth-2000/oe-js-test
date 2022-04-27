<template>
  <div class="store-container">
    <div class="column-manage">
      <div class="container-list">
        <div class="title"><span>Milk Tea Store</span></div>
        <div class="store-list">
          <div
            v-for="store in stores.stores"
            :key="store.id"
            class="store-list-items"
            @click="changeStore(store.id, store.name)"
            :class="{ backgroundChoice: storeID === store.id }"
          >
            {{ store.name }}
          </div>
        </div>
      </div>
    </div>
    <div class="column-content">
      <StoreView
        :title="title"
        :storeID="storeID"
      />
    </div>
  </div>
</template>
<script>
import stores from "../data/stores.json";
import products from "../data/products.json";
import StoreView from "./StoreView.vue";
export default {
  data() {
    return {
      stores: stores,
      title: stores.stores[0].name,
      productLists: products.products,
      storeMenu: [],
      storeID: 0,
    };
  },
  components: {
    StoreView,
  },
  
  created() {
    this.storeID = stores.stores[0].id;
  },
  methods: {
    changeStore(id, name) {
      this.storeID = id;
      this.title = name;
    },
    
  },
};
</script>
<style scoped>
.store-container {
  width: 1000px;
  margin: 0 auto;
  height: auto;
  display: flex;
}
.column-manage {
  background: #06035d;
  font-size: 18px;
}
.column-manage .container-list {
  width: 200px;
}
.column-manage .title {
  color: white;
  margin: 40px 0 50px;
}
.store-list-items {
  color: white;
  margin-bottom: 10px;
  padding: 20px 0;
  opacity: 0.5;
}
.store-list-items:hover {
  cursor: pointer;
}
.column-content {
  width: 700px;
  background: #cdd9cd;
}
.backgroundChoice {
  background: #013178;
  opacity: 1;
}
@media only screen and (max-width: 768px) {
  .store-container {
    display: block;
    width: 100%;
  }
  .column-manage {
    padding: 10px 0;
  }
  .column-content {
    width: 100%;
  }
  .column-manage .container-list {
    width: 100%;
    text-align: center;
  }
  .column-manage .title {
    margin: 0;
  }
  .store-list {
    display: flex;
    justify-content: space-around;
  }
  .store-list-items {
    padding: 20px;
  }
}
</style>
