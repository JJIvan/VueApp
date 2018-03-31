<template>
	<div class="goods">
		<!-- 分類 -->
		<div class="menu-wrapper" ref="menuScroll">
			<ul>


				<!-- 專場 -->
				<li class="menu-item food-list-hook">
					<p class="text">
						<img :src="container.tag_icon" v-if="container.tag_icon" class="icon">
						{{container.tag_name}}
					</p>
				</li>


				<!--  -->
				<li class="menu-item food-list-hook" v-for="item in goods">
					<p class="text">
						<img :src="item.icon" v-if="item.icon" class="icon">
						{{item.name}}
					</p>
				</li>


			</ul>
		</div>
		<!-- 商品列表 -->
		<div class="foods-wrapper" ref="foodScroll">
			<ul>


				<!-- 專場 -->
				<li class="container-list">
					<div v-for="item in container.operation_source_list">
						<img :src="item.pic_url">
					</div>
				</li>



				<!-- 具體分類 -->
				<li v-for="item in goods" class="food-list">

					<h3 class="title">{{item.name}}</h3>

					<ul>
						<li v-for="food in item.spus" class="food-item">
							<div class="icon" :style="head_bg(food.picture)"></div>
							<div class="content">
								<h3 class="name">{{food.name}}</h3>
								<p class="desc" v-if="food.description">{{food.description}}</p>
								<div class="extra">
									<span class="saled">{{food.month_saled_content}}</span>
									<span class="praise">{{food.praise_content}}</span>
								</div>
								<img class="product" :src="food.product_label_picture">
								<p class="price">
									<span class="text">${{food.min_price}}</span>
									<span class="unit">/{{food.unit}}</span>
								</p>
							</div>
						</li>
					</ul>
				</li>



			</ul>
		</div>



	</div>
</template>
<script>
	import BScroll from 'better-scroll';

	export default{
		data(){
			return {
				container:{},
				goods:[],
				listHeight:[]
			}
		},
		created(){   //ajax

		    var that =this;

		    this.$axios.get('/api/goods')
		        .then(function (response) {
		          var dataSource = response.data;
		          if(dataSource.code == 0){
					that.container=dataSource.data.container_operation_source;
					that.goods=dataSource.data.food_spu_tags;  

					that.$nextTick( ()=>{
						that.initScroll();

						that.calculateHeight();

					} ) 
		          }
		        })
		        .catch(function (error) {
		          console.log(error);
		        });
 		 },
 		 methods:{
 		 	head_bg(imgName){
 		 		return "background-image: url("+imgName+")";
 		 	},
 		 	initScroll(){
 		 		new BScroll(this.$refs.menuScroll);
 		 		new BScroll(this.$refs.foodScroll)
 		 	},
 		 	calculateHeight(){
 		 		let foodlist = this.$refs.getElementsByClassName('food-list-hook');
 		 		let height = 0;
 		 		this.listHeight.push(height);
 		 		for(let i=0;i<foodlist.length;i++){
 		 			let item =foodlist[i];
 		 			height += item.clientHeight;
 		 			this.listHeight.push(height)
 		 		}
 		 	}
 		 }
	}
	
</script>
<style>
	@import url("Goods.css");
</style>