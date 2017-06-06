<template>
	<div class="title">
		<div class="line"></div>
		<div class="text">{{text}}</div>
		<div class="line"></div>
	</div>
</template>

<script type="ecmascript-6">
	export default {
		props: ['text']
	}

</script>

<style rel="stylesheet/stylus" lang="stylus">
	.title
		display: flex
		.line
			flex: 1
			position: relative
			top: -6px
			border-bottom: 1px solid rgba(255, 255, 255, 0.2)
		.text
			padding: 0 12px
			font-size: 14px
			line-height: 14px
			font-weight: 700
</style>
