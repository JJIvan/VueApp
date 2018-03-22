<template>
	<div class="header">
		頭部
		{{poiInfo.name}}
	</div>
</template>
<script>
	export default{
		props: {
			poiInfo:{
				type:Object,
				default:{}
			}
		}
	}
</script>
<style>
	
</style>