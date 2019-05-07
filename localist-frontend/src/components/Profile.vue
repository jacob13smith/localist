<template>
    <div class="container">
        <div class="large-12 medium-12 small-12 cell">
            <label>Files
                <input type="file" id="files" ref="files" v-on:change="handleFilesUpload()"/>
            </label>
        </div>
        <div class="large-12 medium-12 small-12 cell">
            <div v-for="(file, key) in files" class="file-listing">{{ file.name }} <span class="remove-file" v-on:click="removeFile( key )">Remove</span></div>
        </div>
        <br>
        <div class="large-12 medium-12 small-12 cell">
            <button v-on:click="addFiles()">Add Files</button>
        </div>
        <br>
        <div class="large-12 medium-12 small-12 cell">
            <button v-on:click="submitFiles()">Submit</button>
        </div>
    </div>
</template>
<script>
    export default {
        /*
          Defines the data used by the component
        */
        data: () =>({
                files: ""

        }),

        /*
          Defines the method used by the component
        */
        methods: {
            /*
              Adds a file
            */
            addFiles(){
                this.$refs.files.click();
            },

            /*
              Submits files to the server
            */
            submitFiles(){
                /*
                  Initialize the form data
                */
                let formData = new FormData();

                /*
                  Iteate over any file sent over appending the files
                  to the form data.
                */
                for( let i = 0; i < this.files.length; i++ ){
                    let file = this.files[i];

                    formData.append('files[' + i + ']', file);
                }

                /*
                  Make the request to the POST /select-files URL
                */
                this.$http
                    .post( '/select-files',
                    formData,
                    {
                        headers: {
                            'Content-Type': 'multipart/form-data'
                        }
                    }
                ).then(function(){
                        console.log('SUCCESS!!');
                })
                    .catch(function(){
                        console.log('FAILURE!!');
                    });
            },

            /*
              Handles the uploading of files
            */
            handleFilesUpload(){
                let uploadedFiles = this.$refs.files.files;

                /*
                  Adds the uploaded file to the files array
                */
                for(let i = 0; i < uploadedFiles.length; i++ ){
                    this.files.push( uploadedFiles[i] );
                }
            },

            /*
              Removes a select file the user has uploaded
            */
            removeFile( key ){
                this.files.splice( key, 1 );
            }
        }
    }
</script>
<style scoped>

</style>