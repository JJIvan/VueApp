<template>
	<div class="test">
		我是測試組件中的內容
		{{msg}}
	</div>
</template>

<script>
export default{
	data(){
		return{
			msg:"我是data屬性中的數據"
		}
	}
}
</script>

<style>
.test{
	color: red;
}
</style>