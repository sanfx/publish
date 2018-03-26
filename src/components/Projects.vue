<template>
<div id="select"><label> Source </label>
    <label>Project</label>
    <select v-model="selected" v-on:change="executeLoader()"># v-bind:disabled="isRunning">
        <option disabled selected>Please Select One</option>
        <!--option></option-->
        <option v-for="n in projects" :value="n">
            {{ n.name }}
        </option>
    </select>
    <div class="round-button">
        <div class="round-button-circle">
            <a href="#">
                <img src="images/leftarrow.png"  alt="Reload" title="Sync with Gitlab" />
            </a>
        </div>
    </div> &nbsp;
    <label>Branch:</label>
    <select v-model="selectedBranch" v-on:change="branchSelecter()">
        <option disabled selected>Select a branch</option>
        <option v-for="b in branches" :value="b">
            {{ b.name }}
        </option>
    </select>
    <br>
    <small>{{ message }}</small>
</div>

</template>

<script>
export default {
  name: 'Projects',
  data: function () {
  return { 
    projects: [],
    branches: [],
    selected: "Please Select One",
    isRunning: false,
    selectedBranch: "Select a branch"
  }
},
  methods : {
   branchSelecter: function(project){
    msg = "Deploy as " + this.selected.name + "@" + this.selectedBranch.name + "-1";
   this.message = "Project: " + this.selected.name + " Branch: "+ this.selectedBranch.name;
   },
   executeLoader: function(){
            baseMsg = "Project: " + this.selected.name;
            this.message = baseMsg + " pulling branches from github."
         if (!this.selected){
            console.log("Nothing is selected");
            this.branches = []

         } else {
         this.branches = ["master"];
         url = "https://api.github.com/repos/sanfx/"+ this.selected.name +"/branches"
         console.log(url);
             axios.get(
                 url,
                     {
                     mode: 'no-cors',
                     headers:
                         {
                             'Accept': 'application/json;charset=UTF-8',
                             'Access-Control-Allow-Origin': '*'
                         }
                     }
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
           console.log(response.data);
         this.projects = response.data;
      })

   }

}
</script>

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
