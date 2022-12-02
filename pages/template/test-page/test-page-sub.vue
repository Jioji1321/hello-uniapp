<template>
	<view>

	</view>
</template>

<script>
	export default {
		onLoad: function(data) {
			//获取main->sub的数据
			console.log('data=',data)
			const eventChannel = this.getOpenerEventChannel();
			//接收sub->main的数据
			eventChannel.emit('acceptData', {data: 'data from sub page'})
			eventChannel.on('acceptData', function(data) {
				console.log(data)
			})
		},
		data() {
			return {

			}
		},
		methods: {

		}
	}
</script>

<style>

</style>
