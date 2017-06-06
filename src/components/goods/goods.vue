<template>
<div>
	<div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
			<ul>
				<li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex === index}" @click="_setHeight(index,$event)">
					<span class="name border-1px"><v-classMap :type="item.type" :show="show" v-show="item.type>0" class="icon"></v-classMap>{{item.name}}</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper" ref="foodWrapper">
			<ul>
				<li class="food-list foodListHock" v-for="item in goods">
					<h1 class="title">{{item.name}}</h1>
					<ul>
						<li class="food-item border-1px" v-for="food in item.foods" @click="foodSelect(food,$event)">
							<div class="food-image">
								<img :src="food.icon" width="57px" height="57px">
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="desc" v-show="food.description">{{food.description}}</p>
								<div class="case">
									<span class="sell-count">月售{{food.sellCount}}份</span>
									<span class="rating">好评率{{food.rating}}%</span>
								</div>
								<div class="price-wrapper">
									<span class="new-price">￥{{food.price}}</span>
									<span class="old-price" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
								</div>
								<div class="cartControl-wrapper">
									<v-cartControl :food="food" @add="addFood"></v-cartControl>	
								</div>
							</div>
						</li>	
					</ul>
				</li>
			</ul>
		</div>
		<v-shopcart :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice" :select-food="selectFood" ref="shopcart"></v-shopcart>
	</div>
	<v-food :food="selectedFood" ref="food" @add="addFood"></v-food>
</div>
</template>

<script type="ecmascript-6">
	import classMap from '@/components/classMap/classMap'
	import BScroll from 'better-scroll'
	import cartControl from '@/components/cartControl/cartControl'
	import shopcart from '@/components/shopcart/shopcart'
	import food from '@/components/food/food'

	const ERR_OK = 0 
	export default{
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				goods: [],
				show: 3,
				listHeight: [],
				scrollY: 0,
				selectedFood: {}
			}
		},
		created() {
			this.$http.get('/api/goods').then((response) => {
				response = response.body
				if (response.errno === ERR_OK) {
					this.goods = response.data
					this.$nextTick(() => {
						this._initScroll()
						this._calculateHeight()
					})
				}
			})
		},
		computed: {
			currentIndex() {
				for (let i = 0; i < this.listHeight.length; i++) {
					let height1 = this.listHeight[i]
					let height2 = this.listHeight[i + 1]
					if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
						return i
					}
				}
				return 0
			},
			selectFood() {
				let foods = []
				this.goods.forEach((good) => {
					good.foods.forEach((food) => {
						if (food.count) {
							foods.push(food)
						}
					})
				})
				return foods
			}
		},
		methods: {
			_initScroll() {
				this.menuScroll = new BScroll(this.$refs.menuWrapper, {
					click: true
				})
				
				this.foodScroll = new BScroll(this.$refs.foodWrapper, {
					click: true,
					probeType: 3
				})
				this.foodScroll.on('scroll', (pos) => {
					this.scrollY = Math.abs(Math.round(pos.y))
				})
			},
			_calculateHeight() {
				let height = 0
				this.listHeight.push(height)
				let foodList = this.$refs.foodWrapper.getElementsByClassName('foodListHock')
				for (let i = 0; i < foodList.length; i++) {
					let item = foodList[i]
					height += item.clientHeight;
					this.listHeight.push(height)
				}
			},
			_setHeight(index, event) {
				if (!event._constructed) {
					return
				}
				let foodList = this.$refs.foodWrapper.getElementsByClassName('foodListHock')
				let el = foodList[index]
				this.foodScroll.scrollToElement(el, 300)
			},
			addFood(target) {
				this.$nextTick(() => {
					this.$refs.shopcart.drop(target)
				})
			},
			foodSelect(food, event) {
				if (!event._constructed) {
					return
				}
				this.selectedFood = food
				this.$refs.food.show()
			}

		},
		components: {
			'v-classMap': classMap,
			'v-cartControl': cartControl,
			'v-shopcart': shopcart,
			'v-food': food
		}
	}
</script>

<style rel="stylesheet/stylus" lang="stylus">
	@import '../../common/stylus/mixin.styl'
	.goods
		display: flex
		position: absolute
		top: 174px
		bottom: 46px
		width: 100%
		overflow: hidden
		.menu-wrapper
			flex: 0 0 80px
			width: 80px
			background: #f3f5f7
			.menu-item
				display: table
				padding: 0 12px
				width: 56px
				line-height: 14px
				height: 54px
				font-size: 0
				&:last-child
					.name
						border-none()
				&.current
					position: relative
					z-index: 10
					margin-top: -1px
					background: #fff
					.name
						font-weight: 700
						border-none()
				.icon
					margin-right: 2px
				.name
					display: table-cell
					vertical-align: middle
					font-size: 12px
					color: rgb(7, 17, 27)
					border-1px(rgba(7, 17, 27, 0.1))
		.foods-wrapper
			flex: 1
			.food-list
				.title
					padding-left: 14px
					border-left: 2px solid #d9dde1
					background: #f3f5f7
					height: 26px
					font-size: 12px
					line-height: 26px
					color: rgb(147, 153, 159)
				.food-item
					display: flex
					margin: 18px
					padding-bottom: 18px
					border-1px(rgba(7, 17, 27, 0.1))
					&:last-child
						border-none()
						margin-bottom: 0
					.food-image
						flex: 0 0 57px
						width: 57px
						margin-right: 10px
					.content
						flex: 1
						.name
							margin-top: 2px
							font-size: 14px
							line-height: 14px
							font-weight: 700
							color: rgb(7, 17, 27)
						.desc
							margin-top: 8px
							font-size: 10px
							line-height: 12px
							color: rgb(147, 153, 159)
						.case
							margin-top: 8px
							font-size: 0
							font-size: 10px
							line-height: 10px
							color: rgb(147, 153, 159)
							.sell-count
								padding-right: 12px
						.price-wrapper
							line-height: 24px
							.new-price
								font-size: 14px
								font-weight: 700
								color: rgb(240, 20, 20)
							.old-price
								font-size: 10px
								color: rgb(147, 153, 159)
								text-decoration: line-through
						.cartControl-wrapper
							position: absolute
							right: -6px
							bottom: 12px		
</style>
