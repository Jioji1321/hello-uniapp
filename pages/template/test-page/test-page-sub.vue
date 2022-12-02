<template>
	<view>
		<view>以下内容为main page传递的数据，放在store中</view>
		<view>
			<text>国家：</text>
			<text>{{this.country}}</text>
		</view>
		<view>
			<text>时间：</text>
			<text>{{this.time}}</text>
		</view>
		<view>
			<text>输入的文本：</text>
			<text>{{this.inputText}}</text>
		</view>
		<br>
		<view>富文本输入：</view>
		<view class="container">
			<editor id="editor" class="ql-container" :placeholder="placeholder" @ready="onEditorReady" name="editor"></editor>
			<!-- v-model="formData.editorVal" -->
			<button type="warn" @tap="undo">撤销</button>
		</view>
		<br>
		<view>以下内容为表单内容</view>
		<view>
			<form @submit="formSubmit" @reset="formReset">
				<view>表单中输入的文本：</view>
				<view class="uni-textarea">
					<textarea type="text" placeholder="输入文本" name="input"></textarea>
					<!-- v-model="formData.inputText" -->
				</view>

				<view>滑动选择器：</view>
				<view>
					<slider min="0" max="100" step="5" show-value="true" @change="sliderChange" name="slider"></slider>
					<!-- v-model="formData.sliderVal" -->
				</view>
				<view>
					<button type="default" form-type="submit">submit</button>
					<button type="default" form-type="reset">reset</button>
				</view>
			</form>
		</view>
	</view>
</template>

<script>
	import {
		mapState,
		mapMutations
	} from 'vuex'
	export default {
		computed: {
			...mapState(['checkBoxStatus', 'country', 'time', 'inputText', 'testObj'])
		},
		methods: {
			...mapMutations(['setCheckBoxStatus', 'setCountry', 'setTime', 'setInputText', 'setTestObj'])
		},
		onLoad: function(data) {
			//获取main->sub的数据
			console.log('data from main page=', data)
			const eventChannel = this.getOpenerEventChannel();
			//接收sub->main的数据
			// eventChannel.emit('acceptData', {data: 'data from sub page'})
			// eventChannel.on('acceptData', function(data) {
			// 	console.log(data)
			// });
			console.log(this.inputText)
			console.log(this.time)
			console.log(this.country)
			let obj = {
				inputText: this.inputText,
				time: this.time,
				country: this.country
			}
			this.$store.commit('setTestObj', obj);
			console.log(this.testObj);
		},
		data() {
			return {
				// formData: {
				// 	inputText: null,
				// 	sliderVal: 0,
				// 	editorVal: null
				// }
			}
		},
		methods: {
			onEditorReady() {
				// #ifdef MP-BAIDU
				this.editorCtx = requireDynamicLib('editorLib').createEditorContext('editor');
				// #endif

				// #ifdef APP-PLUS || H5 ||MP-WEIXIN
				uni.createSelectorQuery().select('#editor').context((res) => {
					this.editorCtx = res.context
				}).exec()
				// #endif
			},
			undo() {
				this.editorCtx.undo()
			},
			sliderChange() {

			},
			formSubmit: function(e) {
				console.log('form发生了submit事件，携带数据为：' + JSON.stringify(e.detail.value))
				var formdata = e.detail.value
				uni.showModal({
					content: '表单数据内容：' + JSON.stringify(formdata),
					showCancel: false
				});
			},
			formReset: function(e) {
				console.log('清空数据')
			},
		}
	}
</script>

<style>
	.container {
		padding: 10px;
	}

	#editor {
		width: 100%;
		height: 300px;
		background-color: #CCCCCC;
	}

	button {
		margin-top: 10px;
	}
</style>
