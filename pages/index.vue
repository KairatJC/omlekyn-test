<template>
  <section class="container">
    <goods />
    <cart />
  </section>
</template>

<script>
import goods from "@/components/goods";
import cart from "@/components/cart";
import { mapActions } from "vuex";

export default {
  components: {
    goods,
    cart,
  },
  beforeMount() {
    this.save_data_to_store();
    console && console.log("%c+", 'font-size: 1px; padding: 40px 87px; line-height: 0; background: url("https://i.ibb.co/FYm176F/dev.gif"); background-size: 175px; color: transparent;');
  },
  methods: {
    ...mapActions({
      save_data_to_store: "get_all_data",
    }),
  },
};
</script>

<style>
.container {
  width: 1080px;
  display: flex;
  margin: 0 auto;
}
@media (max-width: 1080px) {
  .container {
    width: 100%;
  }
}
</style>