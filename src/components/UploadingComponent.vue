<template>
    <div>
      <div class="border shadow-md rounded-lg upload-box pl-6 py-6">
          <div class="upload-text">
              Uploading...
          </div>
          <div class="pt-8">
            <Loader />
          </div>
      </div>
    </div>
</template>

<script>
import Loader from "./Loader";
export default {
    components:{
        Loader
    }
}
</script>

<style>
.upload-text{
    font-weight: 500;
    font-size: 18px;
    color: #4F4F4F;
}

.upload-box{
    width: 400.36px;
    height: 143.57px;
}
</style>