<template>
  <view class="drop_down_component">
    <view class="button_click_show" @click="handleClick">{{
      open ? "收起" : "下拉"
    }}</view>
    <view :class="{'drop_down_panel': true, 'drop_down_panel--close': !open}" :style="{height: styleHeight+'px'}">
      <view class="drop_down_panel_body" :id="`ele_${uid}`">
		<slot></slot>
	  </view>
    </view>
  </view>
</template>

<script>
	import uuid from 'uuid';
	export default {
		props: {
			open: {
				default: false,
			}
		},
		data() {
			return {
				styleHeight: '',
				height: 0,
				uid: ''
			};
		},
		methods: {
			animateHeight() {
				const startHeight = this.open ? this.height : 0;
				const endHeight = this.open ? 0 : this.height;
				this.styleHeight = startHeight;
				console.log(this.open);
				setTimeout(() => {
					this.styleHeight = endHeight;
					this.$emit('change-fn');
				}, 100);
			},
			handleClick() {
				this.animateHeight();
			},
		},
		created() {
			this.uid = uuid();
		},
		mounted() {
			uni.createSelectorQuery().in(this)
				.select(`#ele_${this.uid}`)
				.boundingClientRect()
				.exec((res) => {
					this.height = parseInt(res[0].height.toString());
				})
		},
	};
</script>

<style lang="scss">
	.drop_down_component {
		background-color: #eee;
		.button_click_show {
			line-height: 1.7;
			font-size: 26px;
			text-align: center;
			padding: 0 10px;
		}
		.drop_down_panel {
			position: relative;
			overflow: hidden;
			transition: height 500ms;
			&--close {
				height: 0;
			}
		}
	}
</style>