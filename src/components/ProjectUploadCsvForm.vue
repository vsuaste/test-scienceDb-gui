<template>
  <div class="col-xs-5 content">
    <ul v-for="record in errors" v-if="errors" class="list-group">
      <li class="list-group-item">
        <div class="alert alert-danger">
          <h4>Errors for project {{record.record}}</h4>
          <ul>
            <li>{{record.errors.message}}</li>
          </ul>
        </div>
      </li>
    </ul>
    <h4>Upload projects</h4>
      <form id="project-form" enctype="multipart/form-data" novalidate v-on:submit.prevent="onSubmit">

        <div class="form-group">
          <input type="file" id="uploadTableFile" ref="uploadTable" class="form-control">
        </div>

        <button type="submit" class="btn btn-primary">Upload</button>
      </form>
  </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'

export default {
  data() {
    return {
      loading: false,
      error: null,
      errors: null,
    }
  },
  methods: {
    onSubmit() {
      var t = this;
      let query = '';

      if (t.$refs.uploadTable.value.indexOf('.xlsx') > 0) {
        var formElm = "xlsx_file"
        query = `mutation {bulkAddProjectXlsx{ id }}`
      } else {
        var formElm = "csv_file"
        query = `mutation {bulkAddProjectCsv{ id}}`
      }

      try{
        let formData = new FormData();
        let tableFile = document.querySelector('#uploadTableFile');
        if( (tableFile.files[0].size/ 1024*1024) > t.$MAX_UPLOAD_SIZE()){
          throw `File exceeds limit of ${t.$MAX_UPLOAD_SIZE()} MB`
        }
        formData.append(formElm, tableFile.files[0]);
        formData.append('query', query)
        axios.post(this.$baseUrl(), formData,  {
          headers: {
            'Content-Type': 'multipart/form-data',
            'authorization': `Bearer ${t.$getAuthToken()}`,
            'Accept': 'application/graphql'
          }
        }).then(function(response) {
          t.$router.push('/projects')
        }).catch(function(res) {
            if (res.response && res.response.data && res.response.data && Array
              .isArray(res.response.data)) {
              t.errors = res.response.data
            } else {
              var err = (res && res.response && res.response.data && res.response
                .data.message ?
                res.response.data.message : res)
              t.$root.$emit('globalError', err)
              t.$router.push('/')
            }
        })
      }catch(err){
        console.log(err)
        t.$root.$emit('globalError', err)
        t.$router.push('/')
      }
    }
  }
}
</script>
