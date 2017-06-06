<template>
	<transition name="move">
		<div v-show="foodFlog" class="food" ref="foodFlog">
			<div class="food-content">
				<div class="image-header">
					<img :src="food.image">
				</div>
				<div class="content">
					<div class="title">{{food.name}}</div>
					<div class="detail">
						<span class="sell-count">月售{{food.sellCount}}份</span>
						<span class="sell-rating">好评率{{food.rating}}%</span>
					</div>
					<div class="price-wrapper">
						<span class="new-price"><span class="icon">￥</span>{{food.price}}</span>
						<span class="old-price" v-show="food.oldPrice"><span class="icon">￥</span>{{food.oldPrice}}</span>
					</div>
					<div @click.stop.prevent="addFirst" class="enjoy" v-show="!food.count||food.count===0">
						<span class="text">加入购物车</span>
					</div>
					<div class="cartControl-wrapper">
						<v-cartControl :food="food" @add="addFood"></v-cartControl>
					</div>
				</div>
				<v-line></v-line>
				<div class="info" v-show="food.info">
					<h1 class="title">商品介绍</h1>
					<p class="text">{{food.info}}</p>
				</div>
				<v-line v-show="food.info"></v-line>
				<div class="rating">
					<h1 class="title">商品评价</h1>
					<v-ratingSelect @select="selectRating" @toggle="toggleContent" :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings"></v-ratingSelect>
					<div class="ratings-wrapper">
						<ul v-show="food.ratings&&food.ratings.length">
							<li class="rating border-1px" v-for="rating in food.ratings" v-show="needShow(rating.rateType,rating.text)">
								<div class="user">
									<span class="username">{{rating.username}}</span>
									<img :src="rating.avatar" width="12px" height="12px">
								</div>
								<div class="time">{{rating.rateTime | formatDate}}</div>
								<div class="text">
									<i :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></i>{{rating.text}}
								</div>
							</li>
						</ul>
						<div class="no-ratings" v-show="!food.ratings||food.ratings.length===0">
							暂无评论
						</div>
					</div>
				</div>
			</div>
			<div class="back" @click="hide">
				<i class="icon-arrow_lift"></i>
				<span>返回</span>
			</div>
		</div>
	</transition>
</template>

<script type="ecmascript-6">
	import line from '@/components/line/line'
	import BScroll from 'better-scroll'
	import Vue from 'vue'
	import cartControl from '@/components/cartControl/cartControl'
	import ratingSelect from '@/components/ratingSelect/ratingSelect'
	import {formatDate} from '@/common/js/date.js'
	const ALL = 2
	export default {
		props: {
			food: {
				type: Object
			}
		},
		data() {
			return {
				foodFlog: false,
				selectType: ALL,
				onlyContent: true,
				desc: {
					all: '全部',
					positive: '推荐',
					negative: '吐槽'
				}
			}
		},
		methods: {
			show() {
				this.foodFlog = true
				this.selectType = ALL
				this.onlyContent = true
				this.$nextTick(() => {
					if (!this.foodFlogScroll) {
						this.foodFlogScroll = new BScroll(this.$refs.foodFlog, {
							click: true
						})
					} else {
						this.foodFlogScroll.refresh()
					}
				})
			},
			hide() {
				this.foodFlog = false
			},
			addFirst() {
				if (!event._constructed) {
					return
				}
				this.$emit('add', event.target)
				Vue.set(this.food, 'count', 1)
			},
			addFood() {
				this.$emit('add', event.target)
			},
			selectRating(type) {
				this.selectType = type
				this.$nextTick(() => {
					this.foodFlogScroll.refresh()
				})
			},
			toggleContent() {
				this.onlyContent = !this.onlyContent
				this.$nextTick(() => {
					this.foodFlogScroll.refresh()
				})
			},
			needShow(type, text) {
				if (this.onlyContent && !text) {
					return false
				}
				if (this.selectType === ALL) {
					return true
				} else {
					return type === this.selectType
				}
			}
		},
		filters: {
			formatDate (time) {
				let date = new Date(time)
				return formatDate(date, 'yyyy-MM-dd hh:mm')
			}
		},
		components: {
			'v-line': line,
			'v-cartControl': cartControl,
			'v-ratingSelect': ratingSelect
		}
	}
</script>

<style rel="stylesheet/stylus" lang="stylus">
	@import '../../common/stylus/mixin.styl'
	.food
		position: fixed
		left: 0
		top: 0
		bottom: 48px
		z-index: 30
		width: 100%
		background: #fff
		transform: translate3d(0, 0, 0)
		&.move-enter-active, &.move-leave-active
			transition: all 0.4s
		&.move-enter, &.move-leave-active
			transform: translate3d(100%, 0, 0)
		.back
			position: fixed
			left: 10px
			top: 10px
			padding: 8px 8px
			background: rgba(7, 17, 27, 0.6)
			font-size: 12px
			line-height: 12px
			color: #fff
			border-radius: 10%
			.icon-arrow_lift
				font-size: 10px
		.food-content
			.image-header
				position: relative
				width: 100%
				height: 0
				padding-top: 100%
				img
					position: absolute
					top: 0
					left: 0
					width: 100%
					height: 100%
			.content
				position: relative
				padding: 18px
				.title
					margin-bottom: 8px
					font-size: 14px
					line-height: 14px
					font-weight: 700
					color: rgb(7, 17, 27)
				.detail
					margin-bottom: 18px
					font-size: 0px
					line-height: 10px
					color: rgb(147, 153, 159)
					.sell-count
						margin-right: 12px
						font-size: 10px
					.sell-rating
						font-size: 10px
				.price-wrapper
					line-height: 24px
					font-size: 0
					.new-price
						margin-right: 12px
						font-size: 14px
						font-weight: 700
						color: rgb(240, 20, 20)
						.icon
							font-size: 10px
					.old-price
						font-size: 10px
						color: rgb(147, 153, 159)
				.enjoy
					position: absolute
					right: 18px
					bottom: 18px
					z-index: 10
					height: 24px
					line-height: 24px
					padding: 0px 12px
					box-sizing: border-box
					border-radius: 12px
					background: rgb(0, 160, 220)
					font-size: 10px
					color: rgb(255, 255, 255)
				.cartControl-wrapper
					position: absolute
					right: 12px
					bottom: 12px
			.info
				padding: 18px
				.title
					margin-bottom: 6px
					font-size: 14px
					line-height: 14px
					color: rgb(7, 17, 27)
				.text
					padding: 0 8px
					font-size: 12px
					line-height: 24px
					color: rgb(77, 85, 93)
			.rating
				padding-top: 18px
				.title
					margin-left: 18px
					font-size: 14px
					line-height: 14px
					color: rgb(7, 17, 27)
				.ratings-wrapper
					padding: 0 18px
					.rating
						position: relative
						padding: 16px 0
						border-1px(rgba(7, 17, 27, 0.1))
						.user
							position: absolute
							right: 0
							top: 16px
							font-size: 0
							.username
								display: inline-block
								vertical-align: top
								margin-right: 6px
								font-size: 10px
								line-height: 12px
								color: rgb(147, 153, 159)
							img
								display: inline-block
								vertical-align: top
								border-radius: 50%
						.time
							margin-bottom: 6px
							font-size: 10px
							line-height: 12px
							color: rgb(147, 153, 159)
						.text
							line-height: 16px
							font-size: 12px
							color: rgb(7, 17, 27)
							.icon-thumb_up
								margin-right: 4px
								font-size: 12px
								color: rgb(0, 160, 220)
							.icon-thumb_down
								margin-right: 4px
								font-size: 12px
								color: rgb(147, 153, 159)
					.no-ratings
						width: 100%
						height: 64px
						line-height: 64px
						text-align: center
						font-size: 14px
						color: rgb(147, 153, 159)
						
</style>
