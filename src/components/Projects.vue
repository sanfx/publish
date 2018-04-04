<template>
<div id="select"><label> Source </label>
    <label>Project</label>
     <multiselect  
     placeholder="Select a Project" 
     v-model="selected" 
     :options="projects" 
     :searchable="true" 
     :custom-label="customLabel" 
     track-by="name" 
     @input="executeLoader"
     >
         <template slot="singleLabel" slot-scope="{ option }"><strong>{{ option.name }}</strong></template>

    </multiselect>
    <div class="round-button">
        <div class="round-button-circle">
            <a href="#">
                <img src="images/leftarrow.png"  alt="Reload" title="Sync with Github" />
            </a>
        </div>
    </div> &nbsp;
    <label>Branch:</label>
    <multiselect 
    v-model="selectedBranch" 
    :options="branches"
    :max="3"
    :max-height="120"
    :customLabel="customLabel"
    track-by="name"
    @input="branchSelecter">
        <option disabled selected>Select a branch</option>
        <template slot="singleLabel" slot-scope="{ option }">{{ option.name }}</template>
        <!--option v-for="b in branches" :value="b">
            {{ b.name }}
        </option-->
    </multiselect>
    <br>
    <small>{{ message }}</small>
</div>

</template>

<script>
import Vue from 'vue';
import axios from 'axios';
import {bus} from '../main';
import Multiselect from 'vue-multiselect';

Vue.component('multiselect', Multiselect)
export default {
  name: 'Projects',
  components: {
    Multiselect
  },
  data: function () {
  return { 
    projects: [],
    branches: [],
    projectid: "",
    selected: "Please Select One",
    isRunning: false,
    selectedBranch: "Select a branch",
    message: ""
  }
},
  methods : {
    customLabel: function(data){
    // console.log(data);
    return `${data.name}`
    },
   branchSelecter: function(project){
   // var msg = "Deploy as " + this.selected.name + "@" + this.selectedBranch.name + "-1";
   this.message = "Project ID: " + this.selected.id + "-" + this.selected.name + " Branch: "+ this.selectedBranch.name;
   bus.$emit("branchSelected", this.selectedBranch.name)
   },
   executeLoader: function(){
            var baseMsg = "Project ID: " + this.selected.id + " - " + this.selected.name;
            this.message = baseMsg + " pulling branches from github."
         if (!this.selected){
            console.log("Nothing is selected");
            this.branches = []

         } else {
         this.branches = ["master"];
         var url = "https://api.github.com/repos/sanfx/"+ this.selected.name +"/branches"
         this.projectid = this.selected.id;
         // console.log(url);
             axios.get(
                 url
                 )
             .then(response => {
                 this.branches = response.data;
                 if (this.selectedBranch.name == 'undefined'){
                     this.message = baseMsg + " - " + this.selectedBranch;
                 } else {
                     this.message = baseMsg + " Select a branch";
                 }

                 })
              .catch(err => {
              console.log("Axios Error has occurred", err);
              })
            }
         }
      },
   mounted() {
   axios.get("https://api.github.com/users/sanfx/repos")
   .then(response => {
          this.selectedBranch = "";
          this.projects = response.data;
      })

   }

}
</script>
<!-- <style rel="stylesheet" href="vue-multiselect/dist/vue-multiselect.min.css"></style> -->
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 .round-button {
    width:15px;
    display: inline-block;
    vertical-align:middle;
   }
   .round-button-circle {
      width: 100%;
      height:0;
      padding-bottom: 100%;
      border-radius: 50%;
      line-height:50px;
      border:1px solid #cfdcec;
      overflow:hidden;
      background: #4679BD;
      box-shadow: 0 0 5px gray;
   }
   .round-button-circle:hover {
      background:#30588e;
   }

   .round-button img {
      display: block;
      width: 90%; // control image size
      padding: 24%; // control image alignment
      padding-right: 50%;
      height: auto;
   }
</style>


