<template>
  <div class="d-flex justify-content-center">
    <div class="text-center" style="width: 500px">
      <div class="card">
        <div class="card-header">
          <h1 class="text-center">Student Login</h1>
        </div>
        <div class="card-body">
          <form @submit="login">
            <div class="form-group">
              <i class="fas fa-envelope"></i>
              <label for="password">Email</label>
              <input
                type="email"
                class="form-control rounded-pill"
                id="email"
                v-model="email"
              />
            </div>
            <div class="form-group">
              <label for="password">Password</label>
              <input
                type="password"
                class="form-control rounded-pill"
                id="password"
                v-model="password"
              />
            </div>
            <button type="submit" class="btn btn-primary rounded-pill">
              Login
            </button>
          </form>
        </div>
      </div>
    </div>
    <b-toast
      v-model="showToast"
      :title="toastTitle"
      :auto-hide-delay="3000"
      variant="toastVariant"
    ></b-toast>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
import VueCookies from "vue-cookies";

Vue.use(VueCookies);
Vue.use(VueAxios, axios);

export default {
  name: "AdminLoginpage",
  data() {
    return {
      email: "",
      password: "",
      toastVariant: "",
      showToast : false,
      toastTitle:''
    };
  },
  methods: {
    login(e) {
      e.preventDefault(); // Prevent form submission

      axios.get("https://hope1.onrender.com/Teacherlogin", {
      params: {
        email: this.email // Pass the email value as a query parameter
      }
    }).then((response) => {
        
        const data = response.data;

        if (this.email === data[0] && this.password === data[1]) {
          const teacherDetails = {
            id: this.email,
            type:'teacher'
        };
          this.$cookies.set("isLoggedIn", teacherDetails, { expires: "1d" });
          console.log("Teacher login successful");
          this.$router.push("/TeacherHomepage");
          window.location.reload(); // Refresh the page
        }  else {
           this.showToast = true;
          this.toastTitle = 'Invalid credentials'; // Set the error toast title
          this.toastVariant = 'danger';
          console.log("Invalid credentials");
        }
      });
    },
  },
};
</script>
<style scoped>
.card {
    max-width: 100%;
  margin: 20px;
  border-radius: 15px;
  box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.3);
}

.card-header {
  background-color: #3f51b5;
  color: #fff;
  border-radius: 15px 15px 0 0;
}

.card-body {
  padding: 20px 100px;
}

.form-group {
  margin-bottom: 20px;
}

label {
  font-weight: bold;
}
.form-group input {
  border: solid 1px #3f51b5;
}
.btn-primary {
  background-color: #3f51b5;
  border-color: #3f51b5;
  margin-top: 20px;
}
</style>