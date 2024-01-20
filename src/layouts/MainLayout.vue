<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <!-- <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        /> -->

        <q-toolbar-title> Quasar App </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <div class="row justify-between bi-align-center align q-px-sm q-py-md">
        <h4 class="no-margin">Mahsulotlar</h4>

        <q-btn
          class="q-py-sm flex-inline"
          padding="sm"
          color="teal"
          icon="add"
          @click="this.$refs.product.alert = true"
        />
      </div>
      <div v-if="data" class="row q-px-lg">
        <div class="col-3" v-for="item in data" :key="item">
          <q-card
            style="
              width: 98%;
              margin: 0 auto;
              margin-bottom: 12px;
              padding: 0 8px;
            "
          >
            <h4 class="no-margin card-title">{{ item.name_uz }}</h4>
            <div class="flex justify-between">
              <q-card-section>Narxi:</q-card-section>
              <q-card-section>{{ item.cost }}</q-card-section>
            </div>
            <div class="flex justify-between">
              <q-card-section>Manzil:</q-card-section>
              <q-card-section>{{ item.address }}</q-card-section>
            </div>
            <q-card-actions class="flex justify-end q-pb-sm">
              <q-btn
                color="orange"
                icon="edit"
                class="edit-btn"
                @click="
                  this.$refs.product.m_title = 'edit_product';
                  this.$refs.product.alert = true;
                  this.$refs.product.product.id = item.id;
                  this.$refs.product.product.name_uz = item.name_uz;
                  this.$refs.product.product.cost = item.cost;
                  this.$refs.product.product.address = item.address;
                  this.$refs.product.product.product_type_id =
                    item.product_type_id;
                "
              />
              <q-btn
                color="negative"
                icon="delete"
                @click="
                  this.$refs.product_delete.alert = true;
                  this.$refs.product_delete.id = item.id;
                "
              />
            </q-card-actions>
          </q-card>
        </div>
      </div>
    </q-page-container>
  </q-layout>

  <DeleteProduct ref="product_delete" />
  <Product v-if="types" :types="types" ref="product" />
</template>

<script>
import Product from "src/components/Product.vue";
import DeleteProduct from "src/components/DeleteProduct.vue";
import { defineComponent } from "vue";
import baseUrl from "/src/server/baseurl.js";
import axios from "axios";
export default defineComponent({
  name: "MainLayout",
  components: { DeleteProduct, Product },
  data() {
    return {
      data: [],
      loading: false,
      types: [],
      product_id: 0,
      status: false,
    };
  },

  methods: {
    getProducts() {
      axios
        .request({ method: "get", url: `${baseUrl}/api/product` })
        .then((res) => {
          this.data = res.data;
        });
    },
  },
  created() {
    this.getProducts();
  },
});
</script>

<style>
.card-title {
  margin-bottom: 16px !important;
}

.q-card__section--vert {
  padding: 0;
  padding-bottom: 12px;
}

.edit-btn {
  margin-right: 8px !important;
}

.backdrop {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(3px);
  z-index: 1062;
}

.backdrop div {
  padding: 10px;
  border-radius: 50%;
  animation: spinner 1.5s linear infinite;
}

.spinner-1 {
  border: 5px solid transparent;
  border-block-end: 5px solid #bdd7f1;
}

.spinner-2 {
  border: 5px solid transparent;
  border-block-start: 5px solid #97c1ea;
}

.spinner-3 {
  border: 5px solid transparent;
  border-block-end: 5px solid #5c9dde;
}

.spinner-4 {
  border: 5px solid transparent;
  border-block-start: 5px solid #0066cc;
}

@keyframes spinner {
  to {
    transform: rotate(360deg);
  }
}

.loading-enter-from,
.loading-leave-to {
  opacity: 0;
}

.loading-enter-active,
.loading-leave-active {
  transition: opacity 0.25s;
}
</style>
