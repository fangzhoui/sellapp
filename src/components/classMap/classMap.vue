<template>
	<span :class="classType"></span>
</template>

<script type="ecmascript-6">
	export default {
		props: {
			type: {
				type: Number
			},
			show: {
				type: Number
			}
		},
		created() {
			this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
		},
		computed: {
			classType() {
				if (this.show === 1) {
					let type = 'icon1' + ' ' + this.classMap[this.type] 
					return type
				} else if (this.show === 2) {
					let type = 'icon2' + ' ' + this.classMap[this.type] 
					return type
				} else if (this.show === 3) {
					let type = 'icon3' + ' ' + this.classMap[this.type] 
					return type
				} else if (this.show === 4) {
					let type = 'icon4' + ' ' + this.classMap[this.type] 
					return type
				}
			}
		}
	}
</script>

<style rel="stylesheet/stylus" lang="stylus">
	@import '../../common/stylus/mixin.styl'
	.icon1
		display: inline-block
		vertical-align: top
		width: 12px
		height: 12px
		background-size: 12px 12px
		background-repeat: no-repeat
		&.decrease
			bg-image('decrease_1')
		&.discount
			bg-image('discount_1')
		&.guarantee
			bg-image('guarantee_1')
		&.invoice
			bg-image('invoice_1')
		&.special
			bg-image('special_1')
	.icon2
		display: inline-block
		vertical-align: top
		width: 16px
		height: 16px
		background-size: 16px 16px
		background-repeat: no-repeat
		&.decrease
			bg-image('decrease_2')
		&.discount
			bg-image('discount_2')
		&.guarantee
			bg-image('guarantee_2')
		&.invoice
			bg-image('invoice_2')
		&.special
			bg-image('special_2')
	.icon3
		display: inline-block
		vertical-align: top
		width: 12px
		height: 12px
		background-size: 12px 12px
		background-repeat: no-repeat
		&.decrease
			bg-image('decrease_3')
		&.discount
			bg-image('discount_3')
		&.guarantee
			bg-image('guarantee_3')
		&.invoice
			bg-image('invoice_3')
		&.special
			bg-image('special_3')
	.icon4
		display: inline-block
		vertical-align: top
		width: 16px
		height: 16px
		background-size: 16px 16px
		background-repeat: no-repeat
		&.decrease
			bg-image('decrease_4')
		&.discount
			bg-image('discount_4')
		&.guarantee
			bg-image('guarantee_4')
		&.invoice
			bg-image('invoice_4')
		&.special
			bg-image('special_4')
</style>
