<template>
  <div class="card mt-3">
    <div class="card-header">
      <h1>Admission Form</h1>
    </div>
    <div class="card-body">
      <form @submit="submitForm" enctype="multipart/form-data">
        <div class="form-group">
          <label for="fullName">Full Name:</label>
          <input type="text" class="form-control" id="fullName" v-model="fullName" placeholder="Enter your name" required>
        </div>
        <div class="form-group">
          <label for="email">Email:</label>
          <input type="email" class="form-control" id="email" v-model="email" placeholder="Enter your email" required>
        </div>
        <div class="form-group">
          <label for="course">Course:</label>
          <select class="form-control" id="course" v-model="selectedCourse" required>
            <option value="" disabled>Select a course</option>
            <option v-for="course in courses" :value="course" :key="course">{{ course }}</option>
          </select>
        </div>
        <div class="form-group">
          <label for="photo">Photo:</label>
          <input type="file" class="form-control-file" id="photo" accept="image/*" @change="handlePhotoUpload" required>
        </div>
        <div class="form-group">
          <label for="marksheet">HSSC Marksheet:</label>
          <input type="file" class="form-control-file" id="marksheet" accept="image/*" @change="handleMarksheetUpload" required>
        </div>
        <div class="text-center">
          <button type="submit" class="btn btn-primary">Submit</button>
        </div>
      </form>
      <modal name="my-popup">
        <h2>Response Submitted</h2>
        <p>Entrance test timings will be out soon. You will be notified via the provided email.</p>
        <button class="btn btn-primary btn-md rounded pill" @click="closePopup">Close</button>
      </modal>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VModal from 'vue-js-modal';
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios,axios)
Vue.use(VModal);
export default {
  data() {
    return {
      email:'',
      fullName: '',
      selectedCourse: '',
      photo: null,
      marksheet: null,
      courses: []
    };
  },
  mounted() {
    this.fetchCourses();
  },
  methods: {
    openPopup() {
      this.$modal.show('my-popup');
    },
    closePopup() {
      this.$modal.hide('my-popup');
      this.$router.push('/');
    },
    submitForm() {
        const formData = new FormData();
        formData.append('email', this.email);
        formData.append('fullName', this.fullName);
        formData.append('selectedCourse', this.selectedCourse);
        formData.append('photo', this.photo);
        formData.append('marksheet', this.marksheet);
            this.axios.post('https://hope1.onrender.com/enteranceForm',formData)
      .then((res)=>{
        console.warn(res);
        this.openPopup()
      })
    },
    handlePhotoUpload(event) {
      this.photo = event.target.files[0];
    },
    handleMarksheetUpload(event) {
      this.marksheet = event.target.files[0];
    },
     fetchCourses() {
      axios.get('https://hope1.onrender.com/EntranceForm_courses')
        .then(response => {
          this.courses = response.data;
        })
        .catch(error => {
          console.error('Error fetching courses:', error);
        });
    }
  }
};
</script>

<style scoped>
.card {
  font-family: Verdana;
  text-align: start;
  max-width: 500px;
  margin: 0 auto;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.card-header {
  background-color: #f8f9fa;
  text-align: center !important;
  padding: 10px;
}

.card-body {
  padding: 20px;
}
</style>