<template>
	<div class="rating-select">
		<div class="type border-1px">
			<span @click="select(2,$event)" class="all block" :class="{'active':selectType===2}">{{desc.all}}<span class="num">{{ratings.length}}</span></span>
			<span @click="select(0,$event)" class="positive block" :class="{'active':selectType===0}">{{desc.positive}}<span class="num">{{positives.length}}</span></span>
			<span @click="select(1,$event)" class="negative block" :class="{'active':selectType===1}">{{desc.negative}}<span class="num">{{negatives.length}}</span></span>
		</div>
		<div class="switch" :class="{'on':onlyContent}" @click="toggle">
			<i class="icon-check_circle"></i>
			<span class="text">只看有内容的评价</span>
		</div>
	</div>
</template>

<script type="ecmascript-6">
	const POSITIVE = 0
	const NEGATIVE = 1
	const ALL = 2
	export default {
		props: {
			ratings: {
				type: Array,
				default() {
					return []
				}
			},
			selectType: {
				type: Number,
				default: ALL
			},
			onlyContent: {
				type: Boolean,
				default: false
			},
			desc: {
				type: Object,
				default() {
					return {
						all: '全部',
						positive: '满意',
						negative: '不满意'
					}
				}
			}
		},
		methods: {
			select(type, event) {
				if (!event._constructed) {
					return
				}
				this.$emit('select', type)
			},
			toggle(event) {
				if (!event._constructed) {
					return
				}
				this.$emit('toggle')
			}
		},
		computed: {
			positives() {
				return this.ratings.filter((rating) => {
					return rating.rateType === POSITIVE 
				})
			},
			negatives() {
				return this.ratings.filter((rating) => {
					return rating.rateType === NEGATIVE 
				})
			}
		}
	}
</script>

<style rel="stylesheet/stylus" lang="stylus">
	@import '../../common/stylus/mixin.styl'
	.rating-select
		.type
			padding: 18px 0
			margin: 0 18px
			border-1px(rgba(7, 17, 27, 0.1))
			font-size: 0
			.block
				display: inline-block
				padding: 8px 12px
				border-radius: 2px
				font-size: 12px
				line-height: 16px
				color: rgb(77, 85, 93)
				margin-right: 8px
				&:last-child
					margin-right: 0
				&.all
					background: rgba(0, 160, 220, 0.2)
					&.active
						background: rgb(0, 160, 220)
						color: #fff
				&.positive
					background: rgba(0, 160, 220, 0.2)
					&.active
						background: rgb(0, 160, 220)
						color: #fff
				&.negative
					background: rgba(77, 85, 93, 0.2)
					&.active
						background: rgb(77, 85, 93)
						color: #fff
				.num
					margin-left: 2px
					font-size: 8px
		.switch
			padding: 12px 18px
			border-bottom: 1px solid rgba(7, 17, 27, 0.1)
			font-size: 0
			line-height: 24px
			color: rgb(147, 153, 159)
			&.on
				.icon-check_circle
					color: #00c850
			.icon-check_circle
				display: inline-block
				vertical-align: top
				height: 24px
				margin-right: 4px
				font-size: 24px
			.text
				display: inline-block
				vertical-align: top
				height: 24px
				font-size: 12px
				
				
				
			
</style>
