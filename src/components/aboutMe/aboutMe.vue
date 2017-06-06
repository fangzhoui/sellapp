<template>
	<transition name="move">
		<div class="about-me" ref="aboutMe" v-show="showMe">
			<div class="information-wrapper">
				<h1 class="title">基本信息</h1>
				<div class="information">
					<div class="wrapper">
						<span class="option">姓名：</span><span>方舟</span>
					</div>
					<div class="wrapper">
						<span class="option">毕业学校：</span><span>台州学院</span>
					</div>
					<div class="wrapper">
						<span class="option">学历：</span><span>本科</span>
					</div>
					<div class="wrapper">
						<span class="option">籍贯：</span><span>浙江衢州</span>
					</div>
					<div class="wrapper">
						<span class="option">E-mail：</span><span>601064569@qq.com</span>
					</div>
					<div class="wrapper">
						<span class="option">性别：</span><span>男</span>
					</div>
					<div class="wrapper">
						<span class="option">工作经验：</span><span>1年</span>
					</div>
					<div class="wrapper">
						<span class="option">出生年月：</span><span>1992-7-21</span>
					</div>
					<div class="wrapper">
						<span class="option">身高：</span><span>180cm</span>
					</div>
					<div class="wrapper">
						<span class="option">电话：</span><span>18167007297</span>
					</div>
					<div class="avatar">
						<img src="./fz.png" width="126px" height="180px">
					</div>
				</div>
				<h1 class="title">专业技能</h1>
				<div>
					<div class="message">掌握HTML,CSS, JS，jq等前端技术。</div>
					<div class="message">熟悉webpack等主流模板。</div>
					<div class="message">熟悉vue，bootstrap，jQuery Moblie等主流框架</div>
					<div class="message">掌握对各种浏览器的兼容性调试。</div>
					<div class="message">掌握前端页面适配各个手机型号。</div>
				</div>
				<h1 class="title">工作经验</h1>
				<div class="information">
					<div class="wrapper">
						<span class="option">时间：</span><span>2016年3月--2017年4月</span>
					</div>
					<div class="wrapper">
						<span class="option">公司名称：</span><span>芯易科技(北京)有限公司 | 研发部</span>
					</div>
					<div class="wrapper">
						<span class="option">行业类别：</span><span>互联网/餐饮软件</span>
					</div>
					<div class="wrapper">
						<span class="option">工作内容：</span><span>1.
						负责前端页面，包括PC端，移动端，微信场景页面的制作及交互效果的实现。2.负责前后端的数据交互。</span>
					</div>
				</div>
				<h1 class="title">项目经验</h1>
				<div class="responsibility-description">
					<div class="project">芯易软件微信网上订餐公众号开发</div>
					<div class="desc">
						<h2>责任描述:</h2>
						<p class="text">1.负责前端页面的实现及适配各个手机型号。</p>
						<p class="text">2.实现微信订餐前端相关的业务逻辑及数据交互。</p>
						<p class="text">3.负责将常用的功能封装成jquery插件。</p>
						<h2>项目简介:</h2>
						<p class="text">本项目是芯易科技微信订餐的其中一个模板，包括了微信订餐，及会员管理，用户注册为会员后可查询订单记录，积分记录，兑换积分，账户充值等一系列功能。</p>
					</div>
				</div>
				<h1 class="title">自我评价</h1>
				<div class="responsibility-description">
					<div class="desc">
						<p class="text">1.本人性格开朗、稳重、有活力，待人热情、真诚。</p>
						<p class="text">2.工作认真负责，积极主动，能吃苦耐劳，勇于承受压力，勇于创新。</p>
						<p class="text">3.对前端技术有浓厚的兴趣，对自身程序有很高要求。</p>
						<p class="text">4.具备良好的学习、沟通和团队协作能力。</p>
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
	export default {
		components: {
			'v-line': line
		},
		data() {
			return {
				showMe: false
			}
		},
		methods: {
			show() {
				this.showMe = true
				this.$nextTick(() => {
					if (!this.scroll) {
						this.scroll = new BScroll(this.$refs.aboutMe, {
							click: true
						})
					} else {
						this.scroll.refresh()
					}
				})
			},
			hide() {
				this.showMe = false
			}
		}
	}
</script>

<style rel="stylesheet/stylus" lang="stylus">
	.about-me
		position: fixed
		left: 0
		top: 0
		bottom: 0
		z-index: 30
		width: 100%
		background: #fff
		transform: translate3d(0, 0, 0)
		&.move-enter-active, &.move-leave-active
			transition: all 0.4s
		&.move-enter, &.move-leave-active
			transform: translate3d(100%, 0, 0)
		.information-wrapper
			padding: 0px 10px 20px 10px
			.title
				margin: 8px 0
				font-size: 18px
				color: #000
				line-height: 24px
				background: rgba(7, 17, 27, 0.3)
			.information
				position: relative
				.wrapper
					font-size: 14px
					line-height: 14px
					margin: 4px
					.option
						display: inline-block
						width: 70px
						font-weight: 700
				.avatar
					position: absolute
					top: 0
					right: 0
					width: 126px
					height: 180px
					border-radius: 10px
					overflow: hidden
			.message
				margin: 10px
				border-bottom: 1px solid rgba(7, 17, 27, 0.1)
			.responsibility-description
				font-size: 14px
				&:last-child
					margin-bottom: 10px
				.project
					font-weight: 700
					line-height: 16px
				.desc
					line-height: 16px
					h2
						font-weight: 700
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
</style>
