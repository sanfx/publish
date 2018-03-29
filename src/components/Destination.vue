<template>
	<div id="Destination">
		<label>Destination:</label>
		<input  type="text" v-model="branch">
		 <p>Project Id: {{ projectid }}</p>
		</input>
	</div>
</template>

<script>
export default {
	name: 'Destination',
	data : function (){
		return {
			branch: ""
			}
	},
  created: function(){ 
    bus.$on('branchSelected', function (data){
      this.branch = data;
      })
  }
	
}
</script>

