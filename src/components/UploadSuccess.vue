<template>
  <div class="flex justify-center">
    <div class="border shadow-md rounded-lg" style="width:500px; min-height:500px">
      <div class="flex items-center justify-center pt-6">
          <div class="rounded-full h-10 w-10" style="background:#219653">
            <img src="@/assets/check.png">
          </div>
      </div>
      <div class="pt-4 upload-text text-center">Uploaded Successfully!</div>
      <div class="flex justify-center pt-6">
        <div id="upload-box" :style="{ 'background-image': 'url(' + imageLink + ')'}">
        </div>
      </div>

      <div class="flex justify-center pt-4">
        <div class="link-box">
            <span class="pl-2 link">{{text}}</span>
            <span @click="copyLink" class="float-right">
                <copy-to-clipboard :text="imageLink" @copy="copyLink">
                    <div class="button link-text pt-2 cursor-pointer">Copy link</div>
                </copy-to-clipboard>
            </span>
        </div>
      </div>
      <p v-if="linkCopied" class="text-center pt-4 text-xs">Link Copied</p>
    </div>
  </div>
</template>

<script>
import CopyToClipboard from 'vue-copy-to-clipboard'
export default {
  name: 'UploadSuccess',
  props:{
      imageLink:{
          default:""
      }
  },
  components:{
    CopyToClipboard
  },
  data(){
    return{
        url:"",
        linkCopied: false
    }
  },
  methods:{
    copyLink(){
        this.linkCopied = true
    }
  },
  computed:{
      text(){
          return this.imageLink.length > 70 ?  `${this.imageLink.substring(0, 70)}...` : this.imageLink
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #upload-box{
    border-radius: 12px;
    height: 224.4px;
    width: 400px;
    background-size: contain;
  }

  .upload-text{
    font-style: normal;
    font-weight: 500;
    font-size: 18px;
    color: #4F4F4F;
  }

  .link-box{
    background: #F6F8FB;
    border: 1px solid #E0E0E0;
    height: 34px;
    width:400px;
    border-radius: 8px;
  }

  .button{
    width: 74px;
    height: 30px;
    background: #2F80ED;
    border-radius: 8px;
  }

  .link-text{
    font-style: normal;
    font-weight: 500;
    font-size: 8px;
    text-align: center;
    color: #FFFFFF;
  }
  
  .link{
    font-style: normal;
    font-weight: 500;
    font-size: 8px;
    text-align: center;
    color: #4F4F4F;
  }
</style>
