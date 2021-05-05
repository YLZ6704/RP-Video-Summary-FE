<template>
  <div class="container">
    <loading
      :active.sync="isLoading"
      :can-cancel="false"
      :on-cancel="onCancel"
      :is-full-page="fullPage"
    />
    <div class="row">
      <div class="col-6">
        <div class="my-5 shadow rounded p-5">
          <div class="form-group">
            <label for="video">Please upload your video (must be in wav format and less than 50 MB)</label>
            <input
              id="video"
              ref="video"
              type="file"
              class="form-control-file"
              @change="handleVideoUpload()"
            />
          </div>
          <div 
            class="form-group" 
            style="color:red;"
          >
            {{ uploadWarning }}
          </div>
          <div class="form-group">
            <label for="method">Please upload your video (must be in wav format and less than 50 MB)</label>
            <select
              id="method"
              v-model="method"
              class="form-control"
            >
              <option value="sumy_reduction_summary">
                sumy_reduction_summary
              </option>
              <option value="sumy_lsa_summary">
                sumy_lsa_summary
              </option>
              <option value="sumy_lex_rank_summary">
                sumy_lex_rank_summary
              </option>
              <option value="generate_luhn_summary">
                generate_luhn_summary
              </option>
              <option value="TextRank_summarizer">
                TextRank_summarizer
              </option>
            </select>
          </div>
          <div class="form-group">
            <button 
              type="submit" 
              class="btn btn-primary"
              :disabled="!isReady"
              @click="uploadVideo"
            >
              Upload
            </button>
          </div>
        </div>
      </div>
      <div class="col-6">
        <div class="my-5 shadow rounded p-5 bg-secondary">
          <h5>Console</h5>
          {{ response }}
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-12">
        <div class="my-5 shadow rounded p-5">
          <h3>Summary</h3>
          <p v-if="summary != ''">
            {{ summary }}
          </p>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-12">
        <div class="my-5 shadow rounded p-5">
          <h3>Video Text</h3>
          <p v-if="text != ''">
            {{ text }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Loading from 'vue-loading-overlay';
import 'vue-loading-overlay/dist/vue-loading.css';

export default {
  name: "Panel",
  components: {
    Loading
  },
  props: {},
  data() {
    return {
      isReady:false,
      isLoading: false,
      fullPage: true,
      uploadWarning: "",
      text: "",
      video: "",
      response: null,
      summary: "",
      method:"",
    };
  },
  methods: {
    handleVideoUpload() {
      const uploadedFile = this.$refs.video.files;
      const fileExtends = uploadedFile[0].name.split('.').pop();
      const fileSize = uploadedFile[0].size;
      if(fileExtends !== "wav"){
        this.uploadWarning = "Please upload a wav format file";
        this.isReady = false;
      }else if(fileSize > 50000000){
         this.uploadWarning = "Please upload a wav file which is less than 50 MB";
         this.isReady = false;
      }else{
        this.isReady = true;
        this.uploadWarning = "";
        this.video = uploadedFile;
      }
    },
    uploadVideo() {
      var formData = new FormData();
      formData.append("file", this.video[0]);
      formData.append("method", this.method);
      const videoUploadAPI = "http://127.0.0.1:5000/video/";
      //const videoUploadAPI = "https://video-summary-be.herokuapp.com/video/";
      this.isLoading = true;
      console.log(this.video);
      for(let key in formData) {
        console.log(key + " " + formData.key);
      }
      axios
        .post(videoUploadAPI, formData, {
          headers: {
            "Content-Type": "multipart/form-data"
          }
        })
        .then(r => {
          this.response = r.data.res;
          this.text = r.data.text;
          this.summary = r.data.summary;
          this.isLoading = false;
        })
        .catch(e => {
          this.isLoading = false;
          this.response = e;
          console.log(e);
        });
    },
    onCancel() {
      console.log('User cancelled the loader.')
    }
  }
};
</script>

<style scoped>
.form-group {
  text-align: left;
}
</style>
