<template>
  
  <div class="hello">
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
    <form ref="uploadForm" @submit.prevent="submit">
      <input type="file" ref="uploadFile" @change="onFileUpload()" class="form-label" required>
      <button type="button" @click="startUpload" :disabled="loading" class="btn btn-primary btn-inverse">
        <span v-if="loading" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
        <span v-else>UPLOAD</span>
      </button>

      <div v-if="uploadSuccess" class="alert alert-success mt-2 d-flex justify-content-center align-items-center text-center mx-auto" role="alert" style="max-width: 50vw; max-height: 50vh;">
  UPLOAD SUCCESSFUL
</div>

      <div v-if="uploadSuccess" >
        <router-link to="/about" class="btn btn-primary btn-inverse">Next >> </router-link>
       


</div>

    </form>
  </div>
</template>
<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  data: () => ({
    formData: null,
    loading: false,
    uploadSuccess: false, // new data property
  }),
  methods: {
    onFileUpload() {
      let file = this.$refs.uploadFile.files[0];
      this.formData = new FormData();
      this.formData.append("file", file);
    },
    startUpload() {
      this.loading = true; // set loading to true
      axios({
        url: 'http://localhost:8088/demo/upload-demo-data',
        method: 'POST',
        data: this.formData,
        headers: {
          Accept: 'application/json',
          'Content-Type': 'multipart/form-data'
        },
      }).then(response => {
        console.log(JSON.stringify(response.data));
        setTimeout(() => {
          this.loading = false; // set loading back to false after a delay
          this.uploadSuccess = true; 
          this.$emit('param1', response.data.Customers, 'param2', response.data.sheet1);

// set uploadSuccess to true
        }, 1000); // delay for 3 seconds (adjust as needed)
      });
    },
   
  }
}
</script>