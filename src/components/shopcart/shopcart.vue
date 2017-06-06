<template>
	<div class="shopcart">
		<div class="content" @click="toggleList">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{'high-light':totalCount>0}">
						<i class="icon-shopping_cart"></i>
					</div>
					<div class="num" v-show="totalCount>0">{{totalCount}}</div>
				</div>
				<div class="price" :class="{'high-light':totalPrice>0}">￥{{totalPrice}}</div>
				<div class="desc">另需配送费{{deliveryPrice}}元</div>
			</div>
			<div class="content-right" :class="payClass" @click.stop.prevent="pay">{{payDesc}}</div>
		</div>
		<div class="ball-container" v-for="ball in balls">
			<transition name="drop" @before-enter="berforDrop" @enter="dropping" @after-enter="afterDrop">
				<div class="ball" v-show="ball.show">
					<div class="inner inner-hook"></div>
				</div>
			</transition>
		</div>
		<transition name="fold">
			<div class="shopcart-list"  v-show="listShow">
				<div class="list-header clear-fix">
					<h1 class="title">购物车</h1>
					<span class="empty" @click="empty">清空</span>
				</div>
				<div class="list-content" ref="shopcartList">
					<ul>
						<li class="food-list border-1px" v-for="food in selectFood">
							<span class="food-name">{{food.name}}</span>
							<div class="price-wrapper">
								<span class="price">￥{{food.count*food.price}}</span>
							</div>
							<div class="cartcontrol-wrapper">
								<v-cartControl :food="food" @add="drop"></v-cartControl>
							</div>
						</li>
					</ul>
				</div>
			</div>	
		</transition>
		<transition name="fade">
			<div class="list-mask" v-show="listShow" @click="toggleList"></div>
		</transition>
	</div>
</template>

<script type="ecmascript-6">
	import cartControl from '@/components/cartControl/cartControl'
	import BScroll from 'better-scroll'
	export default {
		props:　{
			selectFood: {
				type: Array,
				default() {
					return []
				}
			},
			deliveryPrice: {
				type: Number,
				default: 0
			},
			minPrice: {
				type: Number,
				default: 0
			}
		},
		data() {
			return {
				balls: [
					{
						show: false
					},
					{
						show: false
					},
					{
						show: false
					},
					{
						show: false
					},
					{
						show: false
					}
				],
				dropBall: [],
				fold: true
			}
		},
		computed: {
			totalPrice() {
				let total = 0
				this.selectFood.forEach((food) => {
					total += food.price * food.count
				})
				return total
			},
			totalCount() {
				let count = 0
				this.selectFood.forEach((food) => {
					count += food.count
				})
				return count
			},
			payDesc() {
				if (this.totalPrice === 0) {
					return `￥${this.minPrice}起送`
				} else if (this.totalPrice < this.minPrice) {
					let diff = this.minPrice - this.totalPrice
					return `还差￥${diff}元起送`
				} else {
					return '去结算'
				}
			},
			payClass() {
				if (this.totalPrice < this.minPrice) {
					return 'not-enough'
				}
				return 'enough'
			},
			listShow() {
				if (!this.totalCount) {
					this.fold = true
					return false
				}
				let show = !this.fold
				if (show) {
					this.$nextTick(() => {
						if (!this.scroll) {
							this.scroll = new BScroll(this.$refs.shopcartList, {
								click: true
							})
						} else {
							this.scroll.refresh()
						}
					})
				}
				return show
			}
		},
		methods: {
			drop(el) {
				for (let i = 0; i < this.balls.length; i++) {
					let ball = this.balls[i]
					if (!ball.show) {
						ball.show = true
						ball.el = el
						this.dropBall.push(ball)
						return
					}
				}
			},
			berforDrop(el) {
				let count = this.balls.length
				while (count--) {
					let ball = this.balls[count]
					if (ball.show) {
						let rect = ball.el.getBoundingClientRect()
						let x = rect.left - 32
						let y = -(window.innerHeight - rect.top - 22)
						el.style.display = ''
						el.style.webkitTransform = `translate3d(0, ${y}px, 0)`
						el.style.transform = `translate3d(0, ${y}px, 0)`
						let inner = el.getElementsByClassName('inner-hook')[0]
						inner.style.webkitTransform = `translate3d(${x}px, 0 , 0)`
						inner.style.transform = `translate3d(${x}px, 0, 0)`
					}
				} 
			},
			dropping(el, done) {
				/* eslint-disable no-unused-vars */
				let rf = el.offsetHeight
				this.$nextTick(() => {
					el.style.webkitTransform = 'translate3d(0, 0, 0)'
					el.style.transform = 'translate3d(0, 0, 0)'
					let inner = el.getElementsByClassName('inner-hook')[0]
					inner.style.webkitTransform = 'translate3d(0, 0 , 0)'
					inner.style.transform = 'translate3d(0, 0, 0)'		
					el.addEventListener('transitionend', done)
				})
			},
			afterDrop(el) {
				let ball = this.dropBall.shift()
				if (ball) {
					ball.show = false
					el.style.display = 'none'
				}
			},
			toggleList() {
				if (!this.totalCount) {
					return
				}
				this.fold = !this.fold
			},
			empty() {
				this.selectFood.forEach((food) => {
					food.count = 0
				})
			},
			pay() {
				if (this.totalPrice < this.minPrice) {
					return
				}
				window.alert(`需要支付${this.totalPrice}元`)
			}
		},
		components: {
			'v-cartControl': cartControl
		}
	}
</script>

<style rel="stylesheet/stylus" lang="stylus">
	@import '../../common/stylus/mixin.styl'
	.shopcart
		position: fixed
		bottom: 0
		left: 0
		z-index: 50
		width: 100%
		height: 48px
		.content
			display: flex
			background: #141d27
			color: rgba(255, 255, 255, 0.4)
			line-height: 24px
			.content-left
				flex: 1
				font-size: 0
				.logo-wrapper
					display: inline-block
					vertical-align: top
					position: relative
					top: -10px
					margin: 0 12px
					padding: 6px
					width: 56px
					height: 56px
					box-sizing: border-box
					border-radius: 50%
					background: #141d27
					.logo
						width: 100%
						height: 100%
						text-align: center
						border-radius: 50%
						background: #2b343c
						&.high-light
							background: rgb(0, 160, 220)
							.icon-shopping_cart
								color: #fff
						.icon-shopping_cart
							font-size: 24px
							line-height: 44px
					.num
						position: absolute
						top: 0
						right: 0
						width: 24px
						height: 16px
						line-height: 16px
						text-align: center
						border-radius: 16px
						font-size: 9px
						color: #fff
						box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
						background: rgb(240, 20, 20)		
				.price
					display: inline-block
					vertical-align: top
					margin-top: 12px
					line-height: 24px
					padding-right: 12px
					border-right: 1px solid rgba(255, 255, 255, 0.1)
					font-size: 16px
					font-weight: 700
					&.high-light
						color: #fff
				.desc
					display: inline-block
					vertical-align: top
					margin: 12px
					font-size: 8px
			.content-right
				flex: 0 0 105px
				width: 105px
				padding-top: 12px
				font-size: 12px
				font-weight: 700
				text-align: center
				background: #2b343c
				&.not-enough
					background: #2b343c
				&.enough
					background: #00b43c
					color: #fff
		.ball-container
			.ball
				position: fixed
				left: 32px
				bottom: 22px
				z-index: 200
				&.drop-enter-active
					transition: all 0.4s cubic-bezier(.4,-0.22,.65,.04)
					.inner
						width: 16px
						height: 16px
						border-radius: 50%
						background: rgb(0, 160, 220)
						transition: all 0.4s linear
		.shopcart-list
			position: absolute
			top: 0
			left: 0
			width: 100%
			z-index: -1
			transform: translate3d(0, -100%, 0)
			&.fold-enter-active, &.fold-leave-active
				transition: all 0.4s
			&.fold-enter, &.fold-leave-active
				transform: translate3d(0, 0, 0)
			.list-header
				padding: 0 18px
				height: 40px
				background: #f3f5f7
				border-bottom: 1px solid rgba(7, 17, 27, 0.1)
				line-height: 40px
				.title
					float: left
					font-size: 14px
					color: rgb(7, 17, 27)
				.empty
					float: right
					font-size: 12px
					color: rgb(0, 160, 220)
			.list-content
				max-height: 217px
				padding: 0 18px
				overflow: hidden
				background: #fff
				.food-list
					position: relative
					padding: 12px 0
					box-sizing: border-box
					border-1px(rgba(7, 17, 27, 0.1))
					.food-name
						font-size: 14px
						line-height: 24px
						color: rgb(7, 17, 27)
					.price-wrapper
						position: absolute
						right: 90px
						bottom: 12px
						line-height: 24px
						font-size: 14px
						font-weight: 700
						color: rgb(240, 20, 20)
					.cartcontrol-wrapper
						position: absolute
						right: 0
						bottom: 6px
		.list-mask
			position: fixed
			top: 0
			left: 0
			width: 100%
			height: 100%
			z-index: -10
			background: rgba(7, 17, 27, 0.6)
			opacity: 1
			backdrop-filer: blur(10px)
			&.fade-enter-active, &.fade-leave-active
				transition: all 0.4s
			&.fade-enter, &.fade-leave-active
				opacity: 0
				background: rgba(7, 17, 27, 0)
				backdrop-filer: blur(0)
</style>
