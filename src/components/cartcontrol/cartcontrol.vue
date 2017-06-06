<template>
	<div class="cart-control">
		<transition name="move">
			<div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decrCart($event)">
				<i class="inner icon-remove_circle_outline"></i>
			</div>
		</transition>
		<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
		<div class="cart-add icon-add_circle"  @click.stop.prevent="addCart($event)"></div>
	</div>
</template>

<script type="ecmascript-6">
import Vue from 'vue'
export default {
	props: {
		food: {
			type: Object
		}
	},
	methods: {
		addCart(event) {
			if (!event._constructed) {
				return
			}
			if (!this.food.count) {
				Vue.set(this.food, 'count', 0)
			}
			this.food.count++
			this.$emit('add', event.target)
		},
		decrCart(event) {
			if (!event._constructed) {
				return
			}
			this.food.count--
		}
	}
}
</script>

<style rel="stylesheet/stylus" lang="stylus">
	.cart-control
		font-size: 0
		.cart-decrease, .cart-add
			display: inline-block
			vertical-align: top
			color: rgb(0, 160, 220)
		.cart-decrease
			padding: 6px
			transform: translate3d(0, 0, 0)
			opacity: 1
			.inner
				display: inline-block
				font-size: 24px
				line-height: 24px
				transform: rotate(0)
			&.move-enter, &.move-leave-active
				opacity: 0
				transform: translate3d(24px, 0, 0)
				.inner
					transform: rotate(360deg)
			&.move-enter-active, &.move-leave-active
				transition: all 0.4s
				.inner
					transition: all 0.4s
		.cart-count
			display: inline-block
			vertical-align: top
			font-size: 10px
			line-height: 36px
			width: 12px
			text-align: center
			color: rgb(147, 153, 159)
		.cart-add
			padding: 6px
			font-size: 24px
			line-height: 24px
</style>
