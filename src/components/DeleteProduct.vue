<template lang="">
  <q-dialog v-model="alert">
    <q-card>
      <q-card-section class="row items-center q-pb-none">
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>
      <q-card-section class="card-text">
        <h5 class="no-margin">Mahsulotni o'chirmoqchimisiz?</h5>
      </q-card-section>

      <q-card-actions align="right">
        <q-btn label="Ha" color="primary" @click="deleteItem()" />
        <q-btn label="Yo'q" color="negative" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>
<script>
import axios from "axios";
import baseUrl from "/src/server/baseurl.js";

import { useQuasar } from "quasar";
export default {
  name: "DeleteProduct",
  data() {
    return {
      id: 0,
      alert: false,
    };
  },
  setup() {
    const $q = useQuasar();

    return {
      showNotifs() {
        $q.notify({
          progress: true,
          message: "O'chirildi!",
          color: "teal",
          timeout: 1000,
          position: "top-right",
          multiLine: true,
          avatar: "/src/assets/correct.png",
        });
      },
    };
  },
  methods: {
    deleteItem() {
      axios
        .request({ method: "delete", url: `${baseUrl}/api/product/${this.id}` })
        .then(() => {
          this.alert = false;
          this.showNotifs();
          this.$parent.getProducts();
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style>
.card-text {
  padding: 0 12px 12px 12px !important;
}
</style>
