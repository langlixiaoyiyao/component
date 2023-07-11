<template>
  <view class="drop_down_component">
    <view class="button_click_show" @click="handleClick">{{
      open ? "收起" : "下拉"
    }}</view>
    <view class="drop_down_panel" :style="{height: styleHeight+'px'}">
      <view :class="{drop_down_panel_body : true}" ref="eleBody">
		<slot></slot>
	  </view>
    </view>
  </view>
</template>

<script>
	import uni from '';
	export default {
		props: {
			open: {
			default: false,
			changeFn: () => {},
			},
		},
		data() {
			return {
				complete: true,
				styleHeight: '',
				height: 0,
			};
		},
		methods: {
			animateHeight() {
				const startHeight = open ? height : 0;
				const endHeight = open ? 0 : height;
				this.styleHeight = startHeight;
				setTimeout(() => {
					this.styleHeight = endHeight;
					setTimeout(() => {
						this.styleHeight = '';
					}, 700)
				}, 100)

			},
			handleClick() {
				this.complete = false;
				this.animateHeight();
				changeFn();
			},
		},
		mounted() {
			uni.createSelectorQuery().in(this)
				.select(`#${this.$refs.eleBody.id}`)
				.boundingClientRect()
				.exec((res) => {
					this.height = parseInt(res[0].height.toString());
				})
		},
	};
</script>

<style lang="sass">
	.drop_down_component {
		.button_click_show {
			line-height: 1.7;
			font-size: 26px;
			text-align: center;
			padding: 0 10px;
		}
		.drop_down_panel {
			height: 0;
			overflow: hidden;
			transition: height 500ms cubic-bezier(0.1, 0.7, 1, 0.1);
		}
	}
</style>