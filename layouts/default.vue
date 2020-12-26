<template>
  <div>
    <Nuxt />
    <div>
      <button class="btn logout" @click="_actionLogout">
        <i class="fas fa-sign-out-alt"></i>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Default',
  methods: {
    _actionLogout() {
      localStorage.removeItem('token')
      this.$router.push('/login')
    },
  },
}
</script>

<style>
html {
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}
.logout {
  position: fixed;
  z-index: 999;
  top: 10px;
  right: 5px;
  background-color: #ff6700;
  color: #fff;
  opacity: 0.8;
  transition: all 0.4s ease-in;
  display: block;
}
</style>
