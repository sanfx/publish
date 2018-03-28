<template>
	<div id="Destination">
		<label>Destination:</label>
		<input  v-bind:projectid="projectid" type="text">
		 <p>Project Id: {{ projectid }}</p>
		</input>
	</div>
</template>

<script>
export default {
	name: 'Destination',
	props: ['projectid'],
	data : function (){
		return {
			branch: ""
			}
	}
	
}
</script>

