<template>
	<div class="shopcart">
		<div class="content-left">
			<div class="logo-wrapper">
				<span class="icon-shopping_cart logo"></span>
				<!-- <i class="num">6</i> -->
			</div>
			<div class="desc-wrapper">
				<!-- <p class="total-price">$158</p> -->
				<p class="tip">另需{{shippingfeetip}}</p>
			</div>
		</div>
		<div class="content-right">
			{{minpricetip}}
		</div>
	</div>
</template>


<script>
	export default{
		props:{
			minpricetip:{
				type:String,
				default:''
			},
			shippingfeetip:{
				type:String,
				default:''
			},
			selectFoods:{
				type:Array,
				default(){
					return [];
				}
			}
		},
		computed:{
			// 總個數
			totalCount(){
				let num = 0;
				this.selectFoods.forEach( (food)=>{
					num += food.count;
				} )
				return num;
			},
			// 總金額
			totalPrice(){
				let total = 0;
				this.selectFoods.forEach( (food)=>{
					total += food.min_price * food.count;
				} )
				return total;
			}
		}
	}
</script>


<style>
	@import url(Shopcart.css);
</style>