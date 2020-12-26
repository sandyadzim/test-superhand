<template>
  <section class="full-height">
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <div class="card-custom">
            <div class="px-5 pt-5 pb-3">
              <div class="col-md-12 text-center">
                <img
                  src="https://superhand.co.id/assets/website/images/logo.png"
                  alt="logo"
                  class="img-fluid mb-3"
                />
                <h2 style="font-family: 'Poppins Bold'">List Employees</h2>
                <hr style="background-color: #fff" />
              </div>
              <div class="col-md-12 text-right mb-2">
                <nuxt-link to="/create">
                  <button class="btn btn-custom-2">
                    <i class="fas fa-plus"></i> Add Employee
                  </button>
                </nuxt-link>
              </div>
              <div class="col-md-12">
                <div class="table-responsive">
                  <table class="table table-striped table-bordered">
                    <thead class="text-center text-white">
                      <tr>
                        <th>Name</th>
                        <th>Email</th>
                        <th>Position</th>
                        <th>Description</th>
                        <th>Actions</th>
                      </tr>
                    </thead>
                    <tbody v-if="isLoading === false" class="text-white">
                      <tr v-for="(i, index) in items" :key="index">
                        <td>{{ i.name }}</td>
                        <td>{{ i.email }}</td>
                        <td>{{ i.jabatan }}</td>
                        <td>{{ i.deskripsi }}</td>
                        <td class="text-center">
                          <nuxt-link
                            class="btn btn-sm btn-custom"
                            :to="`/edit/${i.id}?name=${i.name}`"
                          >
                            <i class="far fa-edit"></i>
                          </nuxt-link>
                        </td>
                      </tr>
                    </tbody>
                    <tbody v-else class="text-center text-white">
                      <tr>
                        <td colspan="5">Loading ....</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'Main',
  middleware: 'auth',
  data: () => ({
    token: null,
    items: [],
    isLoading: false,
  }),
  mounted() {
    this.token = localStorage.getItem('token')
    this._actionGetEmployee(this.token)
  },
  methods: {
    async _actionGetEmployee(token) {
      try {
        this.isLoading = true
        const res = await this.$axios.get('karyawan', {
          headers: { Authorization: token },
        })
        this.isLoading = false
        if (res.data.success === 'true') {
          this.items = res.data.data
        }
      } catch (error) {
        this.isLoading = false
        return error
      }
    },
  },
}
</script>

<style>
.full-height {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  /* text-align: center; */
  background-color: #1b262c;
  background-image: url('../assets/img/wave.svg');
  background-repeat: no-repeat;
  background-position: bottom;
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
.btn-custom-2 {
  background-color: #1c490b;
  color: #fff;
  font-family: 'Poppins Bold';
}
.btn-custom-2:hover {
  opacity: 0.8;
  color: #fff;
  transition: 0.4s all ease;
}
.card-custom {
  border-radius: 8px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.4);
  margin-bottom: 5px;
  margin-top: 5px;
  background-color: #75d1ae;
  color: #fff;
  /* width: 600px; */
}
</style>
