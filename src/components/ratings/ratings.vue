<template>
	<div class="ratings" ref="ratings">
		<div class="content-wrapper">
			<div class="overview">
				<div class="overview-left">
					<div class="score">{{seller.score}}</div>
					<div class="desc">综合评分</div>
					<div class="rank-rate">高于周边商家的{{seller.rankRate}}%</div>
				</div>
				<div class="overview-right">
					<div class="score-wrapper">
						<span class="title">服务态度</span>
						<v-star :size="36" :score="seller.serviceScore"></v-star>
						<span class="score">{{seller.serviceScore}}</span>
					</div>
					<div class="score-wrapper">
						<span class="title">美食评价</span>
						<v-star :size="36" :score="seller.foodScore"></v-star>
						<span class="score">{{seller.foodScore}}</span>
					</div>
					<div class="delivery-wrapper">
						<span class="text">送达时间</span>
						<span class="time">{{seller.deliveryTime}}分钟</span>
					</div>
				</div>
			</div>
			<v-line></v-line>
			<v-ratingSelect :select-type="selectType" :only-content="onlyContent" :ratings="ratings" @select="selectRating" @toggle="toggleContent"></v-ratingSelect>
			<div class="ratings-wrapper">
				<ul>
					<li v-for="rating in ratings" class="rating-item border-1px" v-show="needShow(rating.rateType,rating.text)">
						<div class="avatar">
							<img :src="rating.avatar" width="28px" height="28px">
						</div>
						<div class="rating-wrapper">
							<div class="name">{{rating.username}}</div>
							<div class="star-wrapper">
								<v-star :size="24" :score="rating.score"></v-star>
								<span class="deliveryTime" :v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
							</div>
							<p class="text" v-show="rating.text">{{rating.text}}</p>
							<div class="recommend-wrapper" v-show="rating.recommend">
								<i :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></i>
								<span v-for="item in rating.recommend" class="recommend-item">{{item}}</span>
							</div>
							<div class="time">
								{{rating.rateTime | formatDate}}
							</div>
						</div>
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script type="ecmascript-6">
	import star from '@/components/star/star'
	import line from '@/components/line/line'
	import ratingSelect from '@/components/ratingSelect/ratingSelect'
	import BScroll from 'better-scroll'
	import {formatDate} from '@/common/js/date.js'
	const ALL = 2
	const ERR_OK = 0
	export default{
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				ratings: [],
				selectType: ALL,
				onlyContent: true
			}
		},
		created() {
			this.$http.get('/api/ratings').then((response) => {
				response = response.body
				if (response.errno === ERR_OK) {
					this.ratings = response.data
					this.$nextTick(() => {
						this.scroll = new BScroll(this.$refs.ratings, {
							click: true
						})
					})
				}
			})
		},
		methods: {
			selectRating(type) {
				this.selectType = type
				this.$nextTick(() => {
					this.scroll.refresh()
				})
			},
			toggleContent() {
				this.onlyContent = !this.onlyContent
				this.$nextTick(() => {
					this.scroll.refresh()
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
			'v-star': star,
			'v-line': line,
			'v-ratingSelect': ratingSelect
		}
	}
</script>

<style rel="stylesheet/stylus" lang="stylus">
	@import '../../common/stylus/mixin.styl'
	.ratings
		position: absolute
		top: 174px
		left: 0
		bottom: 0
		width: 100%
		overflow: hidden
		.overview
			display: flex
			padding: 18px 0
			.overview-left
				flex: 0 0 137px
				width: 137px
				border-right: 1px solid rgba(7, 17, 27, 0.1)
				text-align: center
				padding: 6px 0
				@media only screen and (max-width: 320px)
					flex: 0 0 120px
					width: 120px
				.score
					margin-bottom: 6px
					font-size: 24px
					line-height: 28px
					color: rgb(255, 153, 0)
				.desc
					margin-bottom: 8px
					font-size: 12px
					line-height: 12px
					color: rgb(7, 17, 27)
				.rank-rate
					font-size: 10px
					line-height: 10px
					color: rgb(147, 153, 159)
			.overview-right
				flex: 1
				padding: 6px 0 6px 24px
				@media only screen and (max-width: 320px)
					padding-left: 5px
				.score-wrapper
					margin-bottom: 8px
					font-size: 0
					.title
						padding-right: 12px
						display: inline-block
						vertical-align: top
						line-height: 18px
						font-size: 12px
						color: rgb(7, 17, 27)
					.star
						display: inline-block
						vertical-align: top
					.score
						display: inline-block
						vertical-align: top
						margin-left: 12px
						font-size: 12px
						line-height: 18px
						color: rgb(255, 153, 0)
				.delivery-wrapper
					font-size: 0
					line-height: 18px
					.text
						font-size: 12px
						color: rgb(7, 17, 27)
					.time
						margin-left: 12px
						font-size: 12px
						color: rgb(147, 153, 159)
		.ratings-wrapper
			padding: 0px 18px
			.rating-item
				display: flex
				padding: 18px 0
				border-1px(rgba(7, 17, 27, 0.1))
				&:last-child
					border-none()
				.avatar
					flex: 0 0 28px
					width: 28px
					img
						border-radius: 50%
				.rating-wrapper
					flex: 1
					position: relative
					margin-left: 12px
					.name
						margin-bottom: 4px
						font-size: 10px
						line-height: 12px
						color: rgb(7, 17, 27)
					.star-wrapper
						font-size: 0
						margin-bottom: 6px
						.star
							display: inline-block
							vertical-align: top
						.deliveryTime
							display: inline-block
							vertical-align: top
							margin-left: 6px
							font-size: 10px
							line-height: 12px
							color: rgb(147, 153, 159)
					.text
						margin-bottom: 8px
						font-size: 12px
						line-height: 18px
						color: rgb(7, 17, 27)
					.recommend-wrapper
						font-size: 0
						line-height: 16px
						.icon-thumb_up, .icon-thumb_down, .recommend-item
							margin: 0 8px 4px 0
							display: inline-block
							font-size: 9px
						.icon-thumb_up
							font-size: 12px
							color: rgb(0, 160, 220)
						.icon-thumb_down
							font-size: 12px
							color: rgb(183, 187, 191)
						.recommend-item
							padding: 0 6px
							border: 1px solid rgba(7, 17, 27, 0.1)
							border-radius: 1px
							color: rgb(147, 153, 159)
							background: #fff
							box-sizing: border-box
					.time
						position: absolute
						right: 0
						top: 0
						font-size: 10px
						line-height: 12px
						color: rgb(147, 153, 159)
								
</style>
