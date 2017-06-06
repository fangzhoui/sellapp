<template>
	<div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img :src="seller.avatar" width="64px" height="64px">
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<h1 class="name">{{seller.name}}</h1>
				</div>
				<div class="desc">{{seller.description}} / {{seller.deliveryTime}}分钟送达</div>
				<div v-if="seller.supports" class="support">
					<v-classMap :type="seller.supports[0].type" :show="show1" class="icon"></v-classMap>
					<span class="text">{{seller.supports[0].description}}</span>
				</div>
				<div class="support-count" v-if="seller.supports"  @click="showDetail">
					<span class="num">{{seller.supports.length}}个</span>
					<i class="icon-keyboard_arrow_right"></i>
				</div>
			</div>
		</div>
		<div class="bulletin-wrapper" @click="showDetail">
			<span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span><i class="icon-keyboard_arrow_right"></i>
		</div>
		<div class="background">
			<img :src="seller.avatar" width="100%">
		</div>
		<transition name="fade">
			<div class="detail" v-show="detailShow">
				<div class="detail-wrapper clearfix">
					<div class="detail-main">
						<h1 class="name">{{seller.name}}</h1>
						<div class="star-wrapper">
							<v-star :size="48" :score="seller.score"></v-star>
						</div>
						<div class="title-wrapper">
							<v-title :text="information"></v-title>
						</div>
						<ul v-if="seller.supports" class="supports">
							<li class="support-item" v-for="item in seller.supports">
								<v-classMap :type="item.type" :show="show2" class="icon"></v-classMap>
								<span class="text">{{item.description}}</span>
							</li>
						</ul>
						<div class="title-wrapper">
							<v-title :text="notice"></v-title>
						</div>
						<div class="bulletin">
							<p class="text">{{seller.bulletin}}</p>
						</div>
					</div>
				</div>
				<div class="detail-close icon-close" @click="hadeDetail"></div>
			</div>
		</transition>
	</div>
</template>

<script type="ecmascript-6">
	import star from '@/components/star/star'
	import title from '@/components/title/title'
	import classMap from '@/components/classMap/classMap'
	export default {
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				detailShow: false,
				information: '商品信息',
				notice: '商家公告',
				show1: 1,
				show2: 2
			}
		},
		created() {
			this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
		},
		methods: {
			showDetail() {
				this.detailShow = true
			},
			hadeDetail() {
				this.detailShow = false
			}
		},
		components: {
			'v-star': star,
			'v-title': title,
			'v-classMap': classMap
		}
	}
</script>

<style lang="stylus" rel="stylesheet/stylus">
	@import "../../common/stylus/mixin.styl"
	.header
		position: relative
		color: #fff
		background-color: rgba(7, 17, 27, 0.5)
		overflow: hidden
		.content-wrapper
			position: relative
			padding: 24px 12px 18px 24px
			font-size: 0
			.avatar
				display: inline-block
				vertical-align: top
				img
					border-radius: 2px
			.content
				display: inline-block
				vertical-align: top
				margin: 0 0 0 16px
				.title
					margin:2px 0 8px 0
					.brand
						display: inline-block
						vertical-align: top
						width: 30px
						height: 18px
						bg-image('brand')
						background-size: 30px 18px
						background-repeat: no-repeat
					.name
						display:inline-block
						margin-left: 6px
						line-height: 18px
						font-size: 16px
						font-weight: bold
				.desc
					margin-bottom: 10px
					font-size: 12px
					line-height: 12px
				.support
					height: 12px
					.text
						display: inline-block
						vertical-align: top
						margin-left: 4px
						font-size: 10px
						line-height: 13px
				.support-count
					position: absolute
					right: 12px
					bottom: 18px
					padding: 0px 8px
					border-radius: 14px
					height: 24px
					background: rgba(0, 0, 0, 0.2)
					font-size: 0
					.num
						margin-right: 2px
						line-height: 26px
						font-size: 10px
					.icon-keyboard_arrow_right
						display: inline-block
						vertical-align: top
						font-size: 10px
						line-height: 24px
		.bulletin-wrapper
			position: relative
			height: 28px
			line-height: 28px
			padding: 0 24px 0 12px
			white-space: nowrap
			overflow: hidden
			text-overflow: ellipsis
			background: rgba(7, 17, 27, 0.2)
			.bulletin-title
				display: inline-block
				margin-top: 7px
				vertical-align: top
				width: 22px
				height: 12px
				bg-image('bulletin')
				background-size: 22px 12px
				background-repeat: no-repeat
			.bulletin-text
				vertical-align: top
				margin: 0 4px
				font-size: 10px
			.icon-keyboard_arrow_right
				position: absolute
				right: 12px
				top: 9px
				font-size: 10px
		.background
			position: absolute
			left: 0
			top: 0
			width: 100%
			height: 100%
			z-index: -1
			filter: blur(10px)
		.detail
			position: fixed
			top: 0
			z-index: 100
			width: 100%
			height: 100%
			overflow: auto
			background: rgba(7, 17, 27, 0.8)
			opacity: 1
			backdrop-filter: blur(5px)
			&.fade-enter, &.fade-leave-active
				opacity: 0
				background: rgba(7, 17, 27, 0)
				backdrop-filter: blur(0px)				
			&.fade-enter-active, &.fade-leave-active
				transition: all 0.4s
			.detail-wrapper
				min-height: 100%
				width: 100%
			.detail-main
				margin: 64px 36px 0 36px
				padding-bottom: 64px
				.name
					line-height: 16px
					font-size: 16px
					font-weight: 700
					text-align: center
				.star-wrapper
					margin-top: 16px
					text-align: center
				.title-wrapper
					margin: 28px 0 24px 0
				.supports
					.support-item
						padding: 0 12px
						margin-bottom: 12px
						font-size: 0
						&:last-child
							margin-bottom: 0
						.icon
							margin-right: 4px
						.text
							line-height: 16px
							font-size: 12px
				.bulletin
					.text
						font-size: 12px
						line-height: 24px
			.detail-close
				position: relative
				width: 32px
				height: 32px
				margin: -64px auto 0 auto
				clear: both
				font-size: 32px
		
</style>
