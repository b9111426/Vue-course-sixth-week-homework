<template>
  <div class="d-flex flex-row">
    <router-link to="/login" class="btn btn-outline-secondary btn-sm mx-1" type="button">
      <i class="fas fa-user"></i>
      登入會員
    </router-link>

    <router-link type="button" to="/cart" class="btn btn-outline-secondary btn-sm mx-1 position-relative ">
      <i class="fa-solid fa-cart-shopping"></i>
      購物車
      <span class="badge rounded-pill bg-danger position-absolute top-0 start-100 translate-middle" v-if="cartData.carts?.length>0">{{cartData.carts?.length}}</span>
    </router-link>
    <button type="button" class="btn btn-outline-secondary btn-sm mx-1">
      <i class="fa-solid fa-comment"></i>
      聯絡我們
    </button>
    <button type="button" class="btn btn-outline-secondary btn-sm mx-1">
      <i class="fa-solid fa-location-dot"></i>
      門市查詢
    </button>
    <button type="button" class="btn btn-outline-secondary btn-sm mx-1">
      <i class="fa-solid fa-magnifying-glass"></i>
      找產品
    </button>
  </div>
</template>

<script>
import emitter from '@/libs/emitter'
export default {
  data() {
    return {
      cartData: {
        carts: []
      }
    }
  },
  methods: {
    getCart() {
      this.$http.get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`)
        .then((res) => {
          this.cartData = res.data.data
        })
    }
  },
  mounted() {
    this.getCart()
    emitter.on('get-cart', () => {
      this.getCart()
    })
  }
}
</script>
