<template>
	<view class="content">
		<view v-if="hasLogin" class="hello">
		    <view class="title" v-show="false">
		        您好 {{userName}}，您已成功登录。
		    </view>
		    <view class="uni-list">
		        <view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(value,key) in listData" :key="key"
		           >
		            <view class="uni-media-list">
		                <view class="uni-media-list-body">
		                    <view class="uni-media-list-text-top">{{value.title}}</view>
		                    <view class="uni-media-list-text-bottom">
		                        <text>{{value.author_name}}</text>
		                        <text>{{value.published_at}}</text>
		                    </view>
		                </view>
		            </view>
		        </view>
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
				listData:[],
				reload:false,
			};
		},
		computed: mapState(['forcedLogin', 'hasLogin', 'userName']),
		onLoad() {
			this.getList();
		},
		onPullDownRefresh() {
			 this.reload = true;
			 this.getList();
		},
		onReachBottom() {
		    this.getList();
		},
		methods:{
			getList:function(){
				var data = {
				    column: "id,post_id,title,author_name,cover,published_at" //需要的字段名
				};
				if (this.last_id) { //说明已有数据，目前处于上拉加载
				    data.minId = this.last_id;
				    data.time = new Date().getTime() + "";
				    data.pageSize = 10;
				}
				uni.request({
					url: 'https://unidemo.dcloud.net.cn/api/news',
					data: data,
					success: (data) => {
						uni.stopPullDownRefresh();
					    if (data.statusCode == 200) {
					        let list = this.setTime(data.data);
					        this.listData = this.reload ? list : this.listData.concat(list);
					        this.last_id = list[list.length - 1].id;
					        this.reload = false;
					    }
					},
					fail: (data, code) => {
					    console.log('fail' + JSON.stringify(data));
					}
				})
			},
			setTime: function(items) {
			    var newItems = [];
			    items.forEach((e) => {
			        newItems.push({
			            author_name: e.author_name,
			            cover: e.cover,
			            id: e.id,
			            post_id: e.post_id,
			            published_at: "",
			            title: e.title
			        });
			    });
			    return newItems;
			}
		}
	}
</script>

<style>
.hello {
        display: flex;
        flex: 1;
        flex-direction: column;
    }

    .title {
        color: #8f8f94;
        margin-top: 50upx;
    }

    .ul {
        font-size: 30upx;
        color: #8f8f94;
        margin-top: 50upx;
    }

    .ul>view {
        line-height: 50upx;
    }
    .uni-list {
        background-color: #FFFFFF;
        position: relative;
        width: 100%;
        display: flex;
        flex-direction: column;
    }

    .uni-list:after {
        position: absolute;
        z-index: 10;
        right: 0;
        bottom: 0;
        left: 0;
        height: 1px;
        content: '';
        -webkit-transform: scaleY(.5);
        transform: scaleY(.5);
        background-color: #c8c7cc;
    }

    .uni-list::before {
        position: absolute;
        z-index: 10;
        right: 0;
        top: 0;
        left: 0;
        height: 1px;
        content: '';
        -webkit-transform: scaleY(.5);
        transform: scaleY(.5);
        background-color: #c8c7cc;
    }

    .uni-list-cell {
        position: relative;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
    }

    .uni-list-cell-hover {
        background-color: #eee;
    }

    .uni-list-cell::after {
        position: absolute;
        z-index: 3;
        right: 0;
        bottom: 0;
        left: 30upx;
        height: 1px;
        content: '';
        -webkit-transform: scaleY(.5);
        transform: scaleY(.5);
        background-color: #c8c7cc;
    }

    .uni-list .uni-list-cell:last-child::after {
        height: 0upx;
    }

    /* 图文列表 */
    .uni-media-list {
        padding: 22upx 30upx;
        box-sizing: border-box;
        display: flex;
        width: 100%;
        flex-direction: row;
    }

    .uni-navigate-right.uni-media-list {
        padding-right: 74upx;
    }

    .uni-pull-right {
        flex-direction: row-reverse;
    }

    .uni-pull-right>.uni-media-list-logo {
        margin-right: 0upx;
        margin-left: 20upx;
    }

    .uni-media-list-logo image {
        height: 100%;
        width: 100%;
    }


    .uni-media-list-text-bottom {
        width: 100%;
        line-height: 30upx;
        font-size: 26upx;
        color: #8f8f94;
    }

    .uni-media-list-logo {
        width: 180upx;
        height: 140upx;
        margin-right: 20upx;
    }

    .uni-media-list-body {
        display: flex;
        flex: 1;
        flex-direction: column;
        justify-content: space-between;
        align-items: flex-start;
        overflow: hidden;
        height: auto;
    }

    .uni-media-list-text-top {
        width: 100%;
        line-height: 36upx;
        font-size: 30upx;
        height: 74upx;
        font-size: 28upx;
        overflow: hidden;
    }

    .uni-media-list-text-bottom {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }
</style>
