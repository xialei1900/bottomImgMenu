<template>
	<view>
		<view class="uni-mask" v-show="show" :style="{top:offsetTop + 'px'}" @click="hide" @touchmove.stop="touchMoveStop"></view>
		<view :class="['uni-popup','uni-popup-bottom']" v-show="show" :style="{height:menuHeight + 'px'}" @touchmove.stop="touchMoveStop">
			<view class="menu-wrap" v-for="(item) in menuArray" v-bind:key="item.name" :style="{width:itemWidth + 'px'}">
				<view class="menu-item-wrp" :style="{width:itemWidth + 'px'}" @click="clickItem(item)">
					<view class="menu-item-icon" :style="{width:itemWidth + 'px'}">
						<image :src="item.iconUrl" :mode="scaleToFill"></image>
					</view>
					<view class="menu-item-name">
						<text>{{item.name}}</text>
					</view>
				</view>
			</view>
			<slot></slot>
		</view>
	</view>
</template>

<script>
export default {
	props: {
		show: {
			type: Boolean,
			default: false
		},
		menuArray: {
			type: Array
		}
	},
	data() {
		let offsetTop = 0;
		//#ifdef H5
		offsetTop = 44;
		//#endif
		return {
			offsetTop: offsetTop,
			isSupport: false,
			scaleToFill:'scaleToFill'
		};
	},
	methods: {
		hide: function() {
			this.$emit('hidePopup');
		},
		touchMoveStop: function(e) {
			e.preventDefault();
		},
		clickItem:function(item){
			uni.navigateTo({
				url: item.toUrl
			});
		}
	},
	computed: {
		menuHeight: function() {
			return Math.ceil(this.menuArray.length / 4) * 80+10;
		},
		itemWidth: function() {
			var screenWidth=0;
			uni.getSystemInfo({
				success:function(e){
					screenWidth = e.screenWidth;
				}
			})
			return (screenWidth) / 4;
		}
	}
};
</script>
<style>
.uni-mask {
	width: 100%;
	height: 100%;
	position: fixed;
	z-index: 998;
	top: 0;
	right: 0;
	bottom: 0;
	left: 0;
	background-color: rgba(0, 0, 0, 0.3);
}

.uni-popup {
	position: fixed;
	width: 100%;
	z-index: 999;
	background-color: #ffffff;
	box-shadow: 0 0 30upx rgba(0, 0, 0, 0.1);
}

.uni-popup-bottom {
	left: 0;
	bottom: 0;
	width: 100%;
	height: 100upx;
	line-height: 100upx;
	text-align: center;
	padding-top: 10px;
}

.menu-wrap {
	float: left;
	padding-top: 10px;
}

.menu-item-wrp {
	height: 70px;
}

.menu-item-icon {
	height: 50px;
}

.menu-item-name {
	height: 20px;
	line-height: 20px;
}

image {
	width: 50px;
	height: 50px;
	border-radius: 5px;
}
</style>
