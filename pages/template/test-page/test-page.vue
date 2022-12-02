<template>
	<view>
		<view class="item">
			<button type="default" @click="clickMe">点我</button>
		</view>
		<view>
			<form @submit="submit" @reset="reset">
				<view class="uni-form-item uni-column">
					<view class="uni-list-cell uni-list-cell-pd">
						<view>当前checkbox的状态为：{{this.checkBoxStatus}}</view>
						<switch type="checkbox" name="checkbox" @change="checkBoxChange" />
					</view>
				</view>
				<view>
					<view>
						<text>信息：{{this.inputText}}</text>
					</view>
					<view>
						<input class="uni-input" focus placeholder="输入文本信息" @input="inputChange" />
					</view>
				</view>
				<view>
					<view class="title">当前选中的国家为： {{country}}</view>
					<view>
						<radio-group class="uni-list" @change="radioGroupChange">
							<label class="uni-list-cell uni-list-cell-pd" v-for="item in redioItem" :key="item.name">
								<view>
									<radio :id="item.name" :value="item.value" />
								</view>
								<view>
									<label :for="item.name">
										<text>{{item.name}}</text>
									</label>
								</view>
							</label>
						</radio-group>
					</view>
				</view>
				<view>
					<view class="uni-list-cell-left">
						<text>当前选择时间为:</text>
					</view>
					<view class="uni-list-cell-db">
						<picker mode="time" :value="time" start="00:00" end="23:59" @change="changeTime">
							<view class="uni-input">{{this.time}}</view>
						</picker>
					</view>
				</view>
			</form>
		</view>
		<view>
			<button type="default" @click="navigateToPage">点击跳转页面</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				checkBoxStatus: false,
				country: null,
				time: null,
				inputText: null,
				redioItem: [{
						name: "USA",
						value: "美国"
					},
					{
						name: "CHN",
						value: "中国"
					},
				]
			}
		},
		methods: {
			clickMe(e) {
				console.log(e);
				console.log('123');
			},
			inputChange(e) {
				console.log(e);
				this.inputText = e.detail.value;
			},
			submit() {

			},
			reset() {

			},
			checkBoxChange(e) {
				console.log(e);
				this.checkBoxStatus = e.detail.value;
			},
			radioGroupChange(e) {
				console.log(e);
				this.country = e.detail.value;
			},
			changeTime(e) {
				console.log(e);
				this.time = e.detail.value;
			},
			navigateToPage(e) {
				console.log(e);
				uni.navigateTo({
					//main->sub,传输的数据拼装在url后
					url: './test-page-sub?name=xiaowang&age=20&location=SH',
					events: {
						acceptData: function(data) {
							console.log(data);
						}
					},
					success: function(res) {
						res.eventChannel.emit('acceptData', {
							data: 'data from main page'
						})
					}
				})
			}
		},
	}
</script>

<style>

</style>
