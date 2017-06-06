<template>
	<div id="app">
		<v-header :seller="seller"></v-header>
		<div class="nav border-1px">
			<div class="nav-item">
				<router-link to="/goods">商品</router-link>
			</div>
			<div class="nav-item">
				<router-link to="/ratings">评论</router-link>
			</div>	
			<div class="nav-item">
				<router-link to="/seller">商家</router-link>
			</div>
		</div>
		<keep-alive>
			<router-view :seller="seller"></router-view>
		</keep-alive>
	</div>
</template>

<script type="ecmascript-6">
	import header from '@/components/header/header'
	import {urlParse} from '@/common/js/util'
	const ERR_OK = 0
	export default {
		data() {
			return {
				seller: {
					id: (() => {
						let queryParam = urlParse()
						return queryParam.id
					})()
				},
			}
		},
		created() {
			this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
				response = response.body
				if ( response.errno === ERR_OK ) {
					this.seller = Object.assign({}, this.seller, response.data)
				}
			})
		},
		components: {
			"v-header": header
		}
	}
</script>

<style rel="stylesheet/stylus" lang="stylus">
	@import './common/stylus/mixin.styl'
	#app
		.nav
			display: flex
			width: 100%
			height: 40px
			line-height: 40px
			border-1px(rgba(7, 17, 27, 0.1))
			.nav-item
				flex: 1
				text-align: center
				& > a
					display: block
					font-size: 14px
					color: rgb(77, 85, 93)
					&.active
						color: rgb(240, 20, 20)			
</style>
