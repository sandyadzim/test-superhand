<template>
  <section class="full-height">
    <div class="abs">
      <nuxt-link to="/">
        <button class="btn btn-custom">
          <i class="fa fa-chevron-circle-left"></i> Go Back
        </button>
      </nuxt-link>
    </div>
    <div class="card-custom">
      <div class="px-5 pt-5 pb-3">
        <div class="col-md-12 text-center">
          <h2 style="font-family: 'Poppins Bold'">Edit Employees</h2>
          <hr style="background-color: #fff" />
        </div>
        <div class="col-md-12">
          <form class="" @submit.prevent="_actionEdit">
            <div class="form-group text-left">
              <label for="">Name</label>
              <input
                id="name"
                v-model="form.name"
                type="text"
                class="form-control"
                placeholder=""
                required
              />
            </div>
            <div class="form-group text-left">
              <label for="">Email</label>
              <input
                id="email"
                v-model="form.email"
                type="email"
                class="form-control"
                placeholder=""
                required
              />
            </div>
            <div class="form-group text-left">
              <label for="">Position</label>
              <input
                id="jabatan"
                v-model="form.jabatan"
                type="text"
                class="form-control"
                placeholder=""
                required
              />
            </div>
            <div class="form-group text-left">
              <label for="">Description</label>
              <input
                id="deskripsi"
                v-model="form.deskripsi"
                type="text"
                class="form-control"
                placeholder=""
                required
              />
            </div>
            <div class="form-group" style="margin-top: 30px">
              <button v-if="btn_active" class="btn btn-block btn-custom">
                Edit
              </button>
              <button v-else class="btn btn-block btn-secondary" disabled>
                Wait
              </button>
            </div>
          </form>
          <div style="font-family: 'Poppins Bold'">
            <button class="btn btn-block btn-danger" @click="modal = true">
              Delete
            </button>
          </div>
          <b-modal v-model="modal" hide-footer hide-header>
            <div class="text-center">
              <h3>Are you sure?</h3>
            </div>
            <div class="col-md-12 text-center">
              <span class="btn btn-custom" @click="modal = false">No!</span>
              <span
                v-if="btn_del"
                class="btn btn-danger"
                style="font-family: 'Poppins Bold'"
                @click="_actionDelete"
                >Yes!</span
              >
              <span
                v-else
                class="btn btn-secondary"
                style="font-family: 'Poppins Bold'"
                disabled
                >Wait...</span
              >
            </div>
          </b-modal>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'Edit',
  middleware: 'auth',
  data: () => ({
    form: {},
    btn_active: true,
    btn_del: true,
    modal: false,
  }),
  mounted() {
    this._actionGetBefore()
  },
  methods: {
    _actionGetBefore() {
      this.form = {
        name: this.$route.query.name,
      }
    },
    async _actionDelete() {
      try {
        this.btn_del = false
        const id = this.$route.params.id
        const token = localStorage.getItem('token')
        const res = await this.$axios.delete(`karyawan/${id}`, {
          headers: { Authorization: token },
        })
        if (res.data.success === 'true') {
          this.btn_del = true
          this.delToast()
        }
      } catch (error) {
        this.btn_del = true
        alert('Failed')
        return error
      }
    },
    async _actionEdit() {
      try {
        this.btn_active = false
        const token = localStorage.getItem('token')
        const data = {
          id: this.$route.params.id,
          name: this.form.name,
          email: this.form.email,
          jabatan: this.form.jabatan,
          deskripsi: this.form.deskripsi,
        }
        const res = await this.$axios.put('karyawan', data, {
          headers: { Authorization: token },
        })
        if (res.data.success === 'true') {
          this.btn_active = true
          this.makeToast()
          this._actionGetBefore()
        } else {
          this.btn_active = true
          this.failMakeToast()
          this._actionGetBefore()
        }
      } catch (error) {
        this.btn_active = true
        this.failMakeToast()
        this._actionGetBefore()
        return error
      }
    },
    makeToast() {
      this.$bvToast.toast('Edit Employee Success', {
        title: 'Success',
        variant: 'success',
        solid: true,
      })
      setTimeout(() => {
        this.$router.push('/')
      }, 2000)
    },
    failMakeToast() {
      this.$bvToast.toast('Edit Employee Failed', {
        title: 'Failed',
        variant: 'danger',
        solid: true,
      })
    },
    delToast() {
      this.$bvToast.toast('Delete Employee Success', {
        title: 'Success',
        variant: 'success',
        solid: true,
      })
      setTimeout(() => {
        this.$router.push('/')
      }, 1000)
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
.form-group input {
  font-family: 'Poppins Medium';
}
.form-group label {
  font-family: 'Poppins Medium';
}
.abs {
  position: fixed;
  z-index: 999;
  top: 10px;
  left: 5px;
  background-color: #ff6700;
  color: #fff;
  opacity: 0.8;
  transition: all 0.4s ease-in;
  display: block;
}
</style>
