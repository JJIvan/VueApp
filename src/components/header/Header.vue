<template>
	<div class="header">
		頭部
		<div class="top-wrapper">
			<div class="back-wrapper">
				<span class="icon-arrow_lift"></span>
			</div>

			<form class="search-wrapper">
				<span class="search-icon"></span>
				<input class="search-bar" placeholder="搜尋店內商品" type="text" >
			</form>

			<div class="more-wrapper">
				<a class="spelling-bt" href="">拼單</a>
				<div class="more-bt"></div>
			</div>

		</div>
		
		<div class="content-wrapper">
			
		</div>

		<div class="bulletin-wrapper">
			
		</div>

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
    @import url("../../common/styles/icon.css");
	@import url("Header.css");
</style>