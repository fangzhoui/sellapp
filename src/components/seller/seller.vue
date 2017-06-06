<template>
	<div>
		<div class="seller" ref="seller">
			<div class="content">
				<div class="overview-wrapper">
					<div class="overview border-1px">
						<h1 class="name">{{seller.name}}</h1>
						<div class="star-wrapper">
							<v-star :size="36" :score="seller.score"></v-star>
							<span class="count">({{seller.ratingCount}})</span>
							<span class="count">月售{{seller.sellCount}}单</span>
						</div>
					</div>
					<div class="desc-wrapper">
						<div class="wrapper">
							<div class="text">起送价</div>
							<div class="price">
								<span class="big">{{seller.minPrice}}</span>元
							</div>
						</div>
						<div class="wrapper">
							<div class="text">商家配送</div>
							<div class="price">
								<span class="big">{{seller.deliveryPrice}}</span>元
							</div>
						</div>
						<div class="wrapper">
							<div class="text">平均配送时间</div>
							<div class="price">
								<span class="big">{{seller.deliveryTime}}</span>分钟
							</div>
						</div>
					</div>
					<div class="collection" @click="togglleFavorite">
						<i class="icon-favorite" :class="{'active':favorite}"></i>
						<span class="text">{{favoriteText}}</span>
					</div>
				</div>
				<v-line></v-line>
				<div class="supports-wrapper">
					<div class="notice">
						<h1 class="title">公告与活动</h1>
						<p class="text">{{seller.bulletin}}</p>
					</div>
					<ul>
						<li class="item border-1px-top" v-for="item in seller.supports">
							<v-classMap :type="item.type" :show="4"></v-classMap>
							<span class="desc">{{item.description}}</span>
						</li>
					</ul>
				</div>
				<v-line></v-line>
				<div class="pics">
					<h1 class="title">商家实景</h1>
					<div class="pic-wrapper" ref="picWrapper">
						<ul class="pic-list" ref="picList">
							<li class="pic" v-for="pic in seller.pics">
								<img :src="pic" width="120px" height="90px">
							</li>
						</ul>
					</div>
				</div>
				<v-line></v-line>
				<div class="infos-wrapper">
					<h1 class="title">商家信息</h1>
					<div class="infos border-1px-top" v-for="item in seller.infos">{{item}}</div>
				</div>
				<v-line></v-line>
				<div class="aboutMe-wrapper">
					<h1 class="me" @click="showAboutMe">关于我</h1>
				</div>
			</div>
		</div>
		<v-aboutMe ref="aboutMe"></v-aboutMe>
	</div>
</template>

<script type="ecmascript-6">
	import star from '@/components/star/star'
	import line from '@/components/line/line'
	import classMap from '@/components/classMap/classMap'
	import BScroll from 'better-scroll'
	import aboutMe from '@/components/aboutMe/aboutMe'
	import {saveTolocal, loadFromLocal} from '@/common/js/store'
	export default{
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				favorite: (() => {
					return loadFromLocal(this.seller.id, 'favorite', false)
				})(),
				showMe: false
			}
		},
		mounted() {
			this._initScroll()
			this._initpicScroll()
		},
		watch: {
			'seller'() {
				this._initScroll()
				this._initpicScroll()
			}
		},
		computed: {
			favoriteText() {
				return this.favorite ? '已收藏' : '收藏'
			}
		},
		methods: {
			_initScroll() {
				this.$nextTick(() => {
					if (!this.scroll) {
						this.scroll = new BScroll(this.$refs.seller, {
							click: true
						})
					} else {
						this.scroll.refresh()
					}
				})
			},
			_initpicScroll() {
				if (this.seller.pics) {
					let picWidth = 120
					let margin = 6
					let width = (picWidth + margin) * this.seller.pics.length - margin
					this.$refs.picList.style.width = width + 'px'
					this.$nextTick(() => {
						if (!this.picScroll) {
							this.picScroll = new BScroll(this.$refs.picWrapper, {
								scrollX: true,
								eventPassthrough: 'vertical'
							})
						} else {
							this.picScroll.refresh()
						}
					})
				}
			},
			togglleFavorite(event) {
				if (!event._constructed) {
					return
				}
				this.favorite = !this.favorite
				saveTolocal(this.seller.id, 'favorite', this.favorite)
			},
			showAboutMe(event) {
				if (!event._constructed) {
					return
				}
				this.$refs.aboutMe.show()
			}
		},
		components: {
			'v-star': star,
			'v-line': line,
			'v-classMap': classMap,
			'v-aboutMe': aboutMe
		}
	}
</script>

<style rel="stylesheet/stylus" lang="stylus">
	@import '../../common/stylus/mixin.styl'
	.seller
		position: absolute
		top: 174px
		bottom: 0
		left: 0
		width: 100%
		overflow: hidden
		.overview-wrapper
			position: relative
			padding: 0 18px
			.overview
				padding: 18px 0
				border-1px(rgba(7, 17, 27, 0.1))
				.name
					margin-bottom: 8px
					font-size: 14px
					line-height: 14px
					color: rgb(7, 17, 27)
				.star-wrapper
					.star
						display: inline-block
						vertical-align: top
						margin-right: 8px
					.count
						display: inline-block
						vertical-align: top
						font-size: 10px
						line-height: 18px
						color: rgb(77, 85, 93)
						&:first-child
							margin-right: 12px
			.desc-wrapper
				display: flex
				.wrapper
					flex: 1
					text-align: center
					margin: 18px 0
					border-right: 1px solid rgba(7, 17, 27, 0.1)
					&:last-child
						border: none
					.text
						margin-bottom: 4px
						font-size: 10px
						line-height: 10px
						color: rgb(147, 153, 159)
					.price
						line-height: 24px
						font-size: 10px
						color: rgb(7, 17, 27)
						.big
							font-size: 24px
			.collection
				position: absolute
				right: 18px
				top: 18px
				width: 50px
				height: 50px
				text-align: center
				font-size: 0
				.icon-favorite
					display: block
					font-size: 24px
					line-height: 24px
					margin-bottom: 4px
					color: rgb(147, 153, 159)
					&.active
						color: rgb(240, 20, 20)
				.text
					font-size: 10px
					line-height: 10px
					color: rgb(77, 85, 93)
		.supports-wrapper
			padding: 0 18px
			.notice
				padding: 18px 0 16px 0
				.title
					margin-bottom: 8px
					font-size: 14px
					line-height: 14px
					color: rgb(7, 17, 27)
				.text
					padding: 0 12px
					font-size: 12px
					line-height: 24px
					color: rgb(240, 20, 20)
			.item
				padding: 16px 12px
				border-1px-top(rgba(7, 17, 27, 0.1))
				font-size: 0
				.icon4
					margin-right: 6px
				.desc
					font-size: 12px
					line-height: 16px
					color: rgb(7, 17, 27)
		.pics
			padding: 18px 0px 18px 18px
			.title
				margin-bottom: 12px
				font-size: 14px
				line-height: 14px
				color: rgb(7, 17, 27)
			.pic-wrapper
				width: 100%
				overflow: hidden
				white-space: nowrap
				.pic-list
					font-size: 0
					.pic
						display: inline-block
						margin-right: 6px
						width: 120px
						height: 90px
						&:last-child
							margin: 0
		.infos-wrapper
			padding: 18px 18px 0 18px
			.title
				margin-bottom: 12px
				font-size: 14px
				line-height: 14px
				color: rgb(7, 17, 27)
			.infos
				padding: 16px 12px
				font-size: 12px
				line-height: 16px
				color: rgb(7, 17, 27)
				border-1px-top(rgba(7, 17, 27, 0.1))
		.aboutMe-wrapper
			height: 50px
			line-height: 50px
			text-align: center
			.me
				font-size: 16px
				color: rgb(240, 20, 20)
			
</style>
