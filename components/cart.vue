<template>
  <div :class="{ 'cart--opened': cart_opened }" class="cart">
    <h2>Shoping cart</h2>
    <div class="cart__items">
      <template v-if="cart.length != 0">
        <cartItem
          @new_count="new_count()"
          v-for="(item, index) in cart"
          :item="item"
          :ind="index"
          :key="index"
        />
      </template>
      <div v-else class="empty-cart">Empty, nothing here =(</div>
    </div>
    <div class="cart__total">
      Total amount: <span>{{ total }}$.</span>
    </div>

    <button v-if="!cart_opened" class="open-cart" @click="cart_opened = true">
      Open cart
    </button>
    <button v-else class="open-cart" @click="cart_opened = false">
      Close cart
    </button>
  </div>
</template>
<script>
import { mapState } from "vuex";
import cartItem from "~/components/cartItem";

export default {
  data() {
    return {
      total: 0,
      cart_opened: false,
    };
  },
  components: {
    cartItem,
  },
  computed: {
    ...mapState({
      cart: (state) => state.cart,
      currency: (state) => state.currency,
    }),
  },
  watch: {
    currency() {
      this.get_total();
    },
    cart: {
      deep: true,
      handler() {
        this.get_total();
      },
    },
  },
  methods: {
    new_count() {
      this.get_total();
    },
    get_total() {
      let _self = this;
      this.total = this.cart.reduce((acc, curr) => {
        acc += curr.C * curr.counter * _self.currency;
        return parseFloat(acc.toFixed(2));
      }, 0);
    },
  },
};
</script>
<style scoped>
.cart {
  width: 350px;
  position: sticky;
  top: 0;
  height: 50px;
  padding: 25px 20px;
}
.cart__items {
  overflow-y: scroll;
  height: 70vh;
}
h2 {
  font-weight: 400;
  font-size: 18px;
}
.cart__total {
  padding-top: 20px;
}
.empty-cart {
  font-size: 14px;
  color: #777;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100px;
}
.open-cart {
  display: none;
  cursor: pointer;
}
.cart--opened {
  right: 0 !important;
}
@media (max-width: 1080px) {
  .cart {
    position: fixed;
    right: -360px;
    bottom: 0;
    background: #fff;
    transition: 0.1s linear;
    top: 60px;
    height: auto;
    box-shadow: 0px 0px 10px 0 rgba(0, 0, 0, 0.1);
    padding-right: 0;
    width: 320px;
  }
  .open-cart {
    position: fixed;
    left: 0;
    top: 0;
    right: 0;
    width: 100%;
    display: block;
    padding: 20px;
  }
}
</style>