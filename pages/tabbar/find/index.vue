<template>
	<view class="scroll-page">
		<scroll-view id="tab-bar" class="scroll-page-tab" scroll-x :scroll-left="scrollLeft">
			<view
			 	v-for="(tab, index) in tabs"
				:key="tab.id"
				:class="{active: tabIndex == index}"
			 	:id="tab.id"
				:data-current="index"
				@click="tapTab">{{tab.name}}</view>
		</scroll-view>
		<swiper class="scroll-page-main" :current="tabIndex" :duration="300" @change="tabChange">
			<swiper-item v-for="(tab, index1) in tabs" :key="index1">
				<scroll-view class="tab-content" scroll-y>
					<component :is="tab.component"></component>
				</scroll-view>
			</swiper-item>
		</swiper>
	</view>
</template>
<script>

import Course from './course'
import HarwareStore from './hardware-store'
import Land from './land'
import SportStore from './sports-store'
import FoodStore from './food-store'

const WIN_WIDTH = uni.getSystemInfoSync().windowWidth
const tabs = [
	{
		name: '课程与挑战',
		id: 'guanzhu',
		component: Course
	},
	{
		name: '运动商城',
		id: 'tuijian',
		component: SportStore
	},
	{
		name: '健康轻食',
		id: 'tiyu',
		component: FoodStore
	},
	{
		name: '硬件商店',
		id: 'redian',
		component: HarwareStore
	},
	{
		name: 'KeepLand',
		id: 'caijing',
		component: Land
	}
]

	export default {
		data() {
			this.tabs = tabs
			return {
				scrollLeft: 0,
				isClickChange: false,
				tabIndex: 0
			}
		},
		methods: {
			async tabChange(e) {
				const index = e.target.current
				let width = 0
				if (this.isClickChange) {
					this.tabIndex = index
					this.isClickChange = false
					return
				}
				const tabBar = await this.getElSize("tab-bar")
				const	tabBarScrollLeft = tabBar.scrollLeft
				for (let i = 0; i < index; i++) {
					const result = await this.getElSize(this.tabs[i].id)
					width += result.width
				}
					nowElement = await this.getElSize(this.tabs[index].id),
					nowWidth = nowElement.width
				if (width + nowWidth - tabBarScrollLeft > WIN_WIDTH) {
					this.scrollLeft = width + nowWidth - WIN_WIDTH
				}
				if (width < tabBarScrollLeft) {
					this.scrollLeft = width
				}
				this.isClickChange = false
				this.tabIndex = index
			},
			getElSize(id) { //得到元素的size
				return new Promise((res, rej) => {
					uni.createSelectorQuery().select("#" + id).fields({
						size: true,
						scrollOffset: true
					}, (data) => {
						res(data)
					}).exec()
				})
			},
			async tapTab(e) { //点击tab-bar
				let tabIndex = e.target.dataset.current
				if (this.tabIndex === tabIndex) {
					return false
				} else {
					let tabBar = await this.getElSize("tab-bar"),
							tabBarScrollLeft = tabBar.scrollLeft //点击的时候记录并设置scrollLeft
					this.scrollLeft = tabBarScrollLeft
					this.isClickChange = true
					this.tabIndex = tabIndex
				}
			}
		}
	}
</script>

<style lang="scss">
.scroll-page {
	position: absolute;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	overflow: hidden;
	display: flex;
	flex-direction: column;
	// background-color: #333;
	&-main {
		height: 0;
		flex: 1;
	}
	&-tab {
		width: 100%;
		height: 80upx;
		overflow: auto hidden;
		white-space: nowrap;
		font-size: 28upx;
		line-height: 80upx;
		view {
			display: inline-block;
			padding: 0 15px;
			&.active {
				color: red;
			}
		}
	}
}
.tab-content {
	height: 100%;
}
.uni-tab-bar-loading {
	text-align: center;
	font-size: 28upx;
	color: #999;
}
</style>
