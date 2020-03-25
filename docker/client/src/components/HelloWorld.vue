<template>
  <div class="hello">
    <vue-dropzone ref="myVueDropzone" id="dropzone" :options="dropzoneOptions" 
     v-on:vdropzone-sending="sendingEvent"
     v-on:vdropzone-removed-file="removeEvent"
    ></vue-dropzone>
  </div>
</template>

<script>
// vue2-dropzone と vue2-dropzone用のcssをimport
import vue2Dropzone from 'vue2-dropzone'
import 'vue2-dropzone/dist/vue2Dropzone.min.css'
import axios from "axios";

export default {
  name: 'HelloWorld',
  data: function () {
    return {
      dropzoneOptions: {
        url: `http://localhost:8888/images`,
        method: 'post',
	addRemoveLinks: 'true'
      }
    }
  },
  //追加
  methods: {
   sendingEvent: function(file, xhr , formData ) {
    formData.append('uuid', file.upload.uuid )
   },
   removeEvent: function (file) {
   //removeEvent: function (file , error , xhr) {
      axios.delete(`http://localhost:8888/images/${file.upload.uuid}`).then(res => {
        console.log(res.data)
      }).catch(err => {
        console.error(err)
      })
    }

  },
  components: {
    vueDropzone: vue2Dropzone
  }
}
</script>

<!-- CSS 省略 -->
