<template>
  <view class="drop_down_component">
    <view class="button_click_show" @click="handleClick">{{
      open ? "收起" : "下拉"
    }}</view>
    <view class="drop_down_panel">
      <view class="drop_down_panel_body" ref="eleBody">
		<slot></slot>
	  </view>
    </view>
  </view>
</template>

<script>
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
	};
  },
  methods: {
    handleClick() {
      changeFn();
    },
  },
};
</script>

<style lang="sass">
</style>