<template>
	<div id="Destination">
		<label>Destination:</label>
		<input  type="text" v-model="branch">
		 <p>Project Id: {{ branch }}</p>
		</input>
	</div>
</template>

<script>
import {bus} from '../main';
export default {
	name: 'Destination',
	data : function (){
		return {
			branch: ""
			}
	},
  created: function(){ 
    bus.$on('branchSelected',  (data) => {
      this.branch = data;
      })
  }
	
}
</script>

