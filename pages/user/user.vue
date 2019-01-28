<template>
    <view class="content">
        <view class="btn-row">
			<!--  -->			
            <button v-if="!hasLogin" type="primary" class="primary" @tap="bindLogin">登录</button>
			<view v-if="hasLogin" style="padding: 10upx;">
				<view>
					<text >用户名:</text>
					<text v-text="userName"></text>
				</view>
				<view>
					<text >邮箱:</text>
					<text v-text="userName"></text>
				</view>
				 <button  type="default" @tap="bindLogout">退出登录</button>
			</view>
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
            ...mapState(['hasLogin', 'forcedLogin','userName'])
        },
        methods: {
            ...mapMutations(['logout']),
            bindLogin() {
                uni.navigateTo({
                    url: '../login/login',
                });
            },
            bindLogout() {
                this.logout();
                /**
                 * 如果需要强制登录跳转回登录页面
                 */
                if (this.forcedLogin) {
                    uni.reLaunch({
                        url: '../login/login',
                    });
                }
            }
        }
    }
</script>

<style>

</style>
