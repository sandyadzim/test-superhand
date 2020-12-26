<template>
  <section>
    <div class="full-height">
      <div class="card-custom">
        <div class="px-5 pt-5 pb-3">
          <div class="col-md-12 text-center">
            <img
              src="https://superhand.co.id/assets/website/images/logo.png"
              alt="logo"
              class="img-fluid mb-3"
            />
            <h2 style="font-family: 'Poppins Bold'">LOGIN</h2>
          </div>
          <form class="" @submit.prevent="_actionLogin">
            <div class="form-group text-left">
              <label for="" style="font-family: 'Poppins Medium'">Email</label>
              <input
                id="email"
                v-model="form.email"
                type="text"
                class="form-control"
                placeholder=""
                required
                style="font-family: 'Poppins Medium'"
              />
            </div>
            <div class="form-group text-left">
              <label for="" style="font-family: 'Poppins Medium'"
                >Password</label
              >
              <input
                id="password"
                v-model="form.password"
                type="password"
                class="form-control"
                placeholder=""
                required
                autocomplete="false"
                style="font-family: 'Poppins Medium'"
              />
            </div>
            <div class="form-group" style="margin-top: 30px">
              <button v-if="btn_active" class="btn btn-block btn-custom">
                <i class="fa fa-sign-in" /> Masuk
              </button>
              <button v-else class="btn btn-block btn-secondary" disabled>
                <i class="fa fa-sign-in" /> Wait
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'Login',
  layout: 'auth',
  middleware: 'logged',
  data: () => ({
    form: {},
    btn_active: true,
  }),
  methods: {
    async _actionLogin() {
      try {
        this.btn_active = false
        const form = {
          email: this.form.email,
          password: this.form.password,
        }
        const data = await this.$axios.post('login', form)
        if (data.data.success === 'true') {
          this.btn_active = true
          const res = data.data.data
          localStorage.setItem('token', res.token)
          this.$router.push('/')
        }
      } catch (error) {
        this.btn_active = true
        this.makeToast()
        return error
      }
    },
    makeToast() {
      this.$bvToast.toast('Username or Password Wrong!', {
        title: 'Error',
        variant: 'danger',
        solid: true,
      })
    },
  },
}
</script>

<style scoped>
.full-height {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  background-color: #1b262c;
  background-image: url('../../assets/img/wave.svg');
  background-repeat: no-repeat;
  background-position: bottom;
}
.card-custom {
  border-radius: 8px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.4);
  margin-bottom: 5px;
  margin-top: 5px;
  background-color: #75d1ae;
  color: #fff;
  width: 600px;
}
.btn-custom {
  background-color: #ff6700;
  color: #fff;
  font-family: 'Poppins Bold';
}
.btn-custom:hover {
  opacity: 0.8;
  transition: 0.4s all ease;
}
</style>
