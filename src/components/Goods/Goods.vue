<template>
	<div class="goods">
		<!-- 分類 -->
		<div class="menu-wrapper" ref="menuScroll">
			<ul>


				<!-- 專場 -->
				<li class="menu-item" :class="{'current':currendIndex===0}" @click="selectMenu(0)">
					<p class="text">
						<img :src="container.tag_icon" v-if="container.tag_icon" class="icon">
						{{container.tag_name}}
					</p>
				</li>


				<!--  -->
				<li class="menu-item" v-for="(item,index) in goods" :class="{'current':currendIndex===index+1}" @click="selectMenu(index+1)">
					<p class="text">
						<img :src="item.icon" v-if="item.icon" class="icon">
						{{item.name}}
					</p>
					<i class="num" v-show="calculateCount(item.spus)">{{calculateCount(item.spus)}}</i>
				</li>


			</ul>
		</div>
		<!-- 商品列表 -->
		<div class="foods-wrapper" ref="foodScroll">
			<ul>


				<!-- 專場 -->
				<li class="container-list food-list-hook">
					<div v-for="item in container.operation_source_list">
						<img :src="item.pic_url">
					</div>
				</li>



				<!-- 具體分類 -->
				<li v-for="item in goods" class="food-list food-list-hook">

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

							<div class="cartcontrol-wrapper">
								<Cartcontrol :food='food'></Cartcontrol>
							</div>
						</li>
					</ul>
				</li>



			</ul>
		</div>
		<!-- 購物車 -->
		<Shopcart :shippingfeetip="poiInfo.shipping_fee_tip" :minpricetip='poiInfo.min_price_tip' :selectFoods="selectFoods"></Shopcart>



	</div>
</template>
<script>
	import BScroll from 'better-scroll';
	import Shopcart from 'components/Shopcart/Shopcart.vue';
	import Cartcontrol from 'components/Cartcontrol/Cartcontrol.vue';

	export default{
		data(){
			return {
				container:{},
				goods:[],
				poiInfo:{},
				listHeight:[],
				scrollY:0,
				menuScroll:{},
				foodScroll:{}
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
					that.poiInfo=dataSource.data.poi_info;

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
 		 		this.menuScroll = new BScroll(this.$refs.menuScroll,{
 		 			click:true
 		 		});
 		 		this.foodScroll = new BScroll(this.$refs.foodScroll,{
 		 			probeType:3,
 		 			click:true
 		 		});

 		 		this.foodScroll.on('scroll',(pos) => {
 		 			this.scrollY = Math.abs(Math.round(pos.y))
 		 			console.log(this.scrollY)
 		 		})
 		 	},
 		 	calculateHeight(){
 		 		let foodlist = this.$refs.foodScroll.getElementsByClassName('food-list-hook');
 		 		let height = 0;
 		 		this.listHeight.push(height);
 		 		for(let i=0;i<foodlist.length;i++){
 		 			let item =foodlist[i];
 		 			height += item.clientHeight;
 		 			this.listHeight.push(height)
 		 		}
 		 	},
 		 	selectMenu(index){
 		 		let foodlist = this.$refs.foodScroll.getElementsByClassName('food-list-hook');

 		 		let el = foodlist[index];

 		 		this.foodScroll.scrollToElement(el,500);
 		 	},
 		 	calculateCount(spus){
 		 		let count = 0;
 		 		spus.forEach((food)=>{
 		 			if(food.count>0){
 		 				count += food.count;
 		 			};
 		 		});
 		 		return count;
 		 	}
 		 },
 		 computed:{
 		 	currendIndex(){
 		 		for(let i=0;i<this.listHeight.length;i++){
 		 			let height1 = this.listHeight[i];
 		 			let height2 = this.listHeight[i+1];

 		 			if(!height2 || (this.scrollY>=height1 && this.scrollY<height2)){
 		 				return i;
 		 			}

 		 		}
 		 		return 0;

 		 	},selectFoods(){
 		 		let foods = [];
 		 		this.goods.forEach( (good)=>{
 		 			good.spus.forEach( (food)=>{
 		 				if(food.count>0){
 		 					foods.push(food);
 		 					console.log(food)
 		 				}
 		 			});
 		 		});

 		 		return foods;
 		 	}
 		 },
 		 components:{
 		 	Shopcart,
 		 	BScroll,
 		 	Cartcontrol
 		 }

	}
	
</script>
<style>
	@import url("Goods.css");
</style>