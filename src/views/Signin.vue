<template>
  <div class="signin container">
    <h2>Signin</h2>
    <form @submit="signin">
      <div class="form-group">
        <label>Email</label>
        <input type="email" class="form-control" v-model="email" required>
      </div>
      <div class="form-group">
        <label>Password</label>
        <input type="password" class="form-control" v-model="password" required>
      </div>
      <button type="submit" class="btn btn-primary">
        Signin
        <div v-if="loading" class="spinner-border spinner-border-sm" role="status">
          <span class="sr-only">Loading...</span>
        </div>
      </button>
    </form>
    <br/>
    Don't have an account?&nbsp;&nbsp;<router-link :to="{name: 'Signup'}">Signup Here</router-link>
  </div>
</template>

<script>
export default {
  name: 'Signin',
  props : [ "baseURL"],
  data() {
    return {
      email: null,
      password: null,
      loading: null
    }
  },
  methods : {
    async signin(e) {
      e.preventDefault();
      this.loading = true;

      const user = {
        email: this.email,
        password: this.password
      }

      await axios({
        method: 'post',
        url: this.baseURL + "user/signIn",
        data : JSON.stringify(user),
        headers: {
          'Content-Type': 'application/json'
        }
      })
      .then(res => {
        localStorage.setItem('token', res.data.token);
        this.$emit("refreshNav");
        this.$router.back();
      })
      .catch(err => {
        swal({
          text: "Unable to Log you in!",
          icon: "error",
          closeOnClickOutside: false,
        });
        console.log(err);
      })
      .finally(() => {
        this.loading = false;
      })
    }
  },
  mounted() {
    this.loading = false;
  }
}
</script>

<style scoped>
.signin h2{
    text-align : center;
    font-size : 60px;
    margin : 30px 0;
}
.spinner-border {
  margin-left: 10px;
}
</style>