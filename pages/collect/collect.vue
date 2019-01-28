<template>
	<view class="content">
		<view v-if="hasLogin" class="hello">
		    <view class="title">
		        您好 {{userName}}，您已成功登录。
		    </view>
		    <view class="ul">
				
		    </view>
		</view>
		<view v-if="!hasLogin" class="hello">
		    <view class="title">
		        您好 游客。
		    </view>
		    <view class="ul">
		        <view>您当前未登录，无权访问我们的app!</view>
		        <view>在 “我的” 中点击 “登录” 可以 “登录您的账户”</view>
		    </view>
		</view>
	</view>
</template>

<script>
	import {
	    mapState
	} from 'vuex'
	
	export default {
		data() {
			return {
				
			};
		},
		computed: mapState(['forcedLogin', 'hasLogin', 'userName']),
	}
</script>

<style>

</style>
