<template>
	<view class="container">
		<uni-nav-bar right-text="菜单" @clickRight="addDialog">
		    <view id="nav-middle">购物车</view>
		    <view slot="right" id="nav-left"></view>
		</uni-nav-bar>
		<uni-goods-list :cardList="cardList"  title="list"></uni-goods-list>
		<view class="dialog-box" v-show="isDialog">
			<view class="wrapper">
				<view class="bottom-wrap">
					<view class="bottom-box">
						<view class="uni-common-mt bottom-box-item">
							<view class="uni-form-item uni-column">
								<input 
								v-model="inputValue"
								class="uni-input" 
								@input="onKeyInput" placeholder="请输入商品编号" />
							</view>
						</view>
						 <view class="uni-padding-wrap uni-common-mt">
							<button type="primary" @click="addGoodsList()">加入购物车</button>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	import uniGoodsList from "@/components/uni-goods-list/uni-goods-list.vue"
	export default {
		components:{
			uniNavBar,
			uniGoodsList
		},
		data() {
			return {
				inputValue:'',
				isDialog: false,
				title: 'Hello',
				cardList:[
					{goodsId:0, goodsName:"男士衬衫", count:1, price:"200"},
					{goodsId:1, goodsName:"男士衬衫",count:1, price:"200"},
					{goodsId:2, goodsName:"男士衬衫",count:1, price:"200"},
					{goodsId:3, goodsName:"男士衬衫",count:1, price:"200"},
					{goodsId:4, goodsName:"男士衬衫",count:1, price:"200"},
					{goodsId:5, goodsName:"男士衬衫",count:1, price:"200"},
					{goodsId:6, goodsName:"男士衬衫",count:1, price:"200"},
					{goodsId:7, goodsName:"男士衬衫",count:1, price:"200"},
				]
			}
		},
		onLoad() {

		},
		methods: {
			addDialog(){
				this.isDialog=true;
			},
			onKeyInput(e){
				this.inputValue = e.target.value;
			},
			addGoodsList(){
				const goodsId = parseInt(this.inputValue);
				if(!parseInt(goodsId)&& parseInt(goodsId)!=0){
					return this.inputValue=this.inputValue+"不是商品编号，请重新输入"
				}
				let isHave = false;
				//判断下方列表是否存在这个商品
				for(let i=0;i<this.cardList.length;i++){
				  if(this.cardList[i].goodsId == goodsId){
				    isHave = true
					console.log(this.cardList[i].count)
				  }
				}
				//根据isHave的值判断订单列表中是否已经有此商品
				if(isHave){
				  //存在就进行数量增加
				  let arr = this.cardList.filter(o => o.goodsId ===goodsId)
				  //arr是一个新数组，arr[{goodsId:*,goodsName:*,price:*,count:*}],所以访问count需要用arr[0].count访问
				  arr[0].count++;
				  console.log(arr[0])
				}else{
				//不存在就推入数组
				  let newGoods = {goodsId:goodsId,goodsName:"男士衬衫",count:1,price:"200"}
				  this.cardList.push(newGoods)
				}
				this.isDialog=false;
			}
		}
	}
</script>

<style>

	#nav-middle{
		font-size: 30px;
	}
	#nav-left{
		background: url(../../static/assets/group.png);
		display: inline-block;
		height: 22px;
		width: 22px;
	}
	.dialog-box .wrapper{
		position: fixed;
		top: 0;
		left: 0;
		height: 100%;
		width: 100%;
		z-index: 20;
		background: rgba(18, 27, 27, 0.84)
	}
	.dialog-box .wrapper .bottom-wrap{
		background: #FFFFFF;
		position: absolute;
		bottom: 0;
		height: 200px;
		width: 100%;
	}
	.dialog-box .wrapper .bottom-wrap .bottom-box{
		height: 200px;
		width:300px;
		margin: auto;
	}
	.dialog-box .wrapper .bottom-wrap .bottom-box .bottom-box-item{
		margin: 40px 15px;
	}
	.uni-form-item::after{
		content: '';
		height: 1px;
		width: 295px;
		position: absolute;
		background: #d6cece;
		top: 70px;
		left: 40px;
	}
	.bottom-box uni-button[type=primary]{
		background-color: #3366ff;
	}
</style>
