<template>
	<div id="Destination">
		<label>Destination:</label>
		<input v-model="branch" type="text">
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
	}
	
}
</script>

