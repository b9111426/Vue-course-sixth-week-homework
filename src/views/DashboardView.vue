<template>
  <AdminNavbar></AdminNavbar>
  <router-view v-if="checkSuccess"></router-view>
</template>

<script>
import AdminNavbar from '@/components/AdminNavbar.vue'
export default {
  data() {
    return {
      checkSuccess: false
    }
  },
  components: {
    AdminNavbar
  },
  mounted() {
    this.checkLogin()
  },
  methods: {
    checkLogin() {
      const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1')
      if (token) {
        this.$http.defaults.headers.common.Authorization = token
        const api = `${process.env.VUE_APP_API}/api/user/check`
        this.$http.post(api, { api_token: this.token })
          .then(() => {
            this.checkSuccess = true
          })
          .catch((err) => {
            alert(err.data.message)
            this.$router.push('/login')
          })
      } else {
        alert('您尚未登入')
        this.$router.push('/login')
      }
    }
  }
}
</script>
