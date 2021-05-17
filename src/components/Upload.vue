<template>
  <div class="flex justify-center">

  <div v-if="loading" class="pt-24">
    <Uploading/>
  </div>

    <div v-else-if="!loading && !uploaded" class="border shadow-md rounded-lg" style="width:500px; min-height:500px">
      <div class="pt-8 heading text-center">Upload your image</div>
      <div class="pt-4 sub-heading text-center">File should be Jpeg, Png...</div>
      <div class="flex justify-center pt-6" @dragover="dragover"  @drop="drop">
        <div class="upload-box" style="width:400px; min-height:250px">
          <input type="file" name="fields[assetsFieldHandle][]" id="assetsFieldHandle" 
          class="w-px h-px opacity-0 overflow-hidden absolute" @change="handleFileChange" ref="file" accept=".jpg,.jpeg,.png" />

          <div class="flex justify-center pt-10">
            <img src="@/assets/image.svg" width="115" height="88">
          </div>

          <div class="image-text pt-10 text-center">Drag & Drop your image here</div>
        </div>
      </div>
      <div class="image-text pt-8 text-center">Or</div>

      <div @click="uploadFile()" class="flex justify-center pt-6 cursor-pointer">
        <div class="rounded-lg button-text">
          <div class="py-2 text-center">
            Choose a file
          </div>
        </div>
      </div>
    </div>

    <div v-if="uploaded && !loading">
      <UploadSuccess :imageLink="url" />
    </div>
  </div>
</template>

<script>
import Uploading from "./UploadingComponent";
import UploadSuccess from "./UploadSuccess";
import axios from "axios";
export default {
  name: 'HelloWorld',
  data(){
    return{
      filelist: [],
      loading: false,
      uploaded: false,
      fileContents: null,
      cloudName: "dt7xfykxp",
      file: null,
      formData: null,
      preset: "kosvy4q2",
      url:"",
      errors:[]
    }
  },
  components:{
    Uploading,
    UploadSuccess
  },
  methods:{
    uploadFile(){
      document.getElementById('assetsFieldHandle').click()
    },
    onChange() {
      this.file = this.$refs.file.files[0];
      this.filesSelected = this.$refs.file.files.length;
      this.filelist = [...this.$refs.file.files];
      this.upload()
    },
    handleFileChange: function(event) {
      this.file = event.target.files[0];
      this.filesSelected = event.target.files.length;
      this.filelist = [...event.target.files];
      this.upload()
    },
    prepareFormData: function() {
      this.formData = new FormData();
      this.formData.append("upload_preset", this.preset);
      this.formData.append("file", this.fileContents);
    },
    upload: function() {
      this.loading = true
      if (this.preset.length < 1 || this.cloudName.length < 1) {
        this.errors.push("You must enter a cloud name and preset to upload");
        return;
      }
      else {
        this.errors = [];
      }
      let reader = new FileReader();
      reader.addEventListener(
        "load",
        function() {
          this.fileContents = reader.result;
          this.prepareFormData();
          let cloudinaryUploadURL = `https://api.cloudinary.com/v1_1/${this.cloudName}/upload`;
          let requestObj = {
            url: cloudinaryUploadURL,
            method: "POST",
            data: this.formData
          };

          axios(requestObj)
            .then(response => {
              this.results = response.data;
              this.url = this.results.url
              this.uploaded = true
            })
            .catch(error => {
              this.errors.push(error);
              console.log(this.error);
            })
            .finally(() => {
              this.loading = false
            });
        }.bind(this),
        false
      );
      if (this.file && this.file.name) {
        reader.readAsDataURL(this.file);
      }
    },
    remove(i) {
      this.filelist.splice(i, 1);
    },
    dragover(event) {
      event.preventDefault();
    },
    drop(event) {
      event.preventDefault();
      this.$refs.file.files = event.dataTransfer.files;
      this.onChange(); 
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .heading{
    font-style: normal;
    font-weight: 500;
    font-size: 18px;
    letter-spacing: -0.035em;
    color: #4F4F4F;
  }
  .sub-heading{
    font-style: normal;
    font-weight: 500;
    font-size: 10px;
    letter-spacing: -0.035em;
    color: #828282;
  }
  .upload-box{
    background: #F6F8FB;
    border: 1px dashed #97BEF4;
    box-sizing: border-box;
    border-radius: 12px;
  }

  .image-text{
    font-style: normal;
    font-weight: 500;
    font-size: 12px;
    color: #BDBDBD;
  }

  .button-text{
    font-style: normal;
    font-weight: 500;
    font-size: 12px;
    color: #FFFFFF;
    background: #2F80ED;
    width: 101px;
    height: 31.98px;
  }

  input { 
    display: block; 
    visibility: hidden; 
    width: 0; 
    height: 0;
  }
</style>
