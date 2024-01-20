<template lang="">
  <q-dialog v-model="alert">
    <q-card>
      <q-card-section class="row items-center q-pb-none q-pt-sm">
        <h5 class="no-margin">
          Mahsulotni
          {{ m_title == "add_product" ? "qo'shish" : "o'zgartirish" }}
        </h5>
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>
      <form @submit.prevent="addProduct()">
        <q-card-section class="card-text">
          <q-input
            outlined
            v-model="product.name_uz"
            label="Nomi"
            class="q-mb-md"
          />
          <q-input
            outlined
            type="number"
            v-model="product.cost"
            label="Narxi"
            class="q-mb-md"
          />
          <q-input
            outlined
            v-model="product.address"
            label="Manzil"
            class="q-mb-md"
          />
          <select class="q-select" v-model="product.product_type_id">
            <option v-for="item in types" :key="item" :value="item.id">
              {{ item.name_uz }}
            </option>
          </select>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn icon="done" type="submit" color="primary" />
          <q-btn icon="close" type="button" color="negative" v-close-popup />
        </q-card-actions>
      </form>
    </q-card>
  </q-dialog>
</template>
<script>
import axios from "axios";
import baseUrl from "/src/server/baseurl.js";

import { useQuasar } from "quasar";
export default {
  name: "addModal",
  data() {
    return {
      types: [],
      id: 0,
      alert: false,
      m_title: "add_product",
      product: {
        product_type_id: 0,
        name_uz: "",
        cost: null,
        address: "",
        created_date: new Date().toISOString().split("T")[0],
      },
    };
  },
  setup() {
    const $q = useQuasar();

    return {
      add() {
        $q.notify({
          progress: true,
          message: "Qo'shildi!",
          color: "teal",
          timeout: 1000,
          position: "top-right",
          multiLine: true,
          avatar: "/src/assets/correct.png",
        });
      },
      edit() {
        $q.notify({
          progress: true,
          message: "O'zgartirildi!",
          color: "teal",
          timeout: 1000,
          position: "top-right",
          multiLine: true,
          avatar: "/src/assets/correct.png",
        });
      },
    };
  },
  created() {
    this.getTypes();
  },
  methods: {
    addProduct() {
      if (this.m_title == "add_product") {
        axios
          .request({
            method: "post",
            url: `${baseUrl}/api/product`,
            data: this.product,
          })
          .then((res) => {
            if (res.status == 200) {
              this.alert = false;
              this.add();
              this.$parent.getProducts();
              this.product = {
                product_type_id: 0,
                name_uz: "",
                cost: null,
                address: "",
                created_date: new Date().toISOString().split("T")[0],
              };
            }
          })
          .catch((err) => console.log(err));
      } else {
        axios
          .request({
            method: "put",
            url: `${baseUrl}/api/product`,
            data: this.product,
          })
          .then((res) => {
            this.alert = false;
            this.edit();
            this.$parent.getProducts();
            this.product = {
              product_type_id: 0,
              name_uz: "",
              cost: null,
              address: "",
              created_date: new Date().toISOString().split("T")[0],
            };
          })
          .catch((err) => console.log(err));
      }
    },
    getTypes() {
      axios
        .request({
          method: "get",
          url: `${baseUrl}/api/product/get-product-types`,
        })
        .then((res) => {
          this.types = res.data;
        });
    },
  },
};
</script>

<style scoped>
.q-card {
  width: 380px;
  padding: 0 8px !important;
}
.card-text {
  padding: 0 12px 12px 12px !important;
}

.q-select {
  width: 100%;
  padding: 16px 12px;
  border: none;
  border: 1.25px solid rgba(154, 153, 155, 0.48);
  border-radius: 4px;
}

.q-select:focus {
  border: 2px solid #1976d2 !important;
}
.q-select:focus-visible {
  border: 2px solid #1976d2 !important;
}
</style>
