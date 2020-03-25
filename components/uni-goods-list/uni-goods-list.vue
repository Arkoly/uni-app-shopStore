<template>
	<view class="goods-wrapper">
		<view class="card-list" >
			<view class="card-list-item" v-for="(goods,index) in cardList">
				<text class="item-left">{{goods.goodsId}}</text>
				<view class="item-middle">
					<view class="item-title item-middle-list">{{goods.goodsName}}</view>
					<view class="item-middle-list item-box">
						<text class="add-button" @click="addGoods(goods,cardList)"></text>
						<text class="number">{{goods.count}}</text>
						<text class="reduce-button" @click="delGoods(goods,cardList)"></text>
					</view>
				</view>
				<view class="item-right">
					<text class="item-price">￥{{goods.price}}</text>
				</view>
			</view>
		</view>
		<view class="goods-bottom-box">
			<view class="wrapper">
				<view class="goods-bottom-list left-wrap">
					<view class="all-money-box">
						<strong>总价：</strong>
						<text class="all-money">￥{{allMoney}}</text>
					</view>
					<view class="other-tags">包邮</view>
				</view>
				<view class="goods-bottom-list right-wrap">￥{{allMoney}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "UniGoodsList",
		components: {
		},
		data(){
			return{
				totalCount:0,
				totalMoney:0,
			}
		},
		props: {
			cardList:{
				type: Array,
				default: []
			},
			title:{
				type: String,
				default: ""
			}
			
		},
		created(){
			// this.getAllMoney(this.cardList);
		},
		mounted() {
			console.log(this.totalMoney);
		},
		computed:{
			allMoney(){
			  this.totalCount = 0;
			  this.totalMoney = 0;
			  //进行数量和价格的汇总计算
				this.cardList.forEach((element) => {
				  this.totalCount = this.totalCount+element.count;
				  this.totalMoney = this.totalMoney + (element.price*element.count);
				});
				return this.totalMoney
			}
		},
		methods: {
			   addGoods(goods){
			      let isHave = false;
			      //判断下方商品列表中是否存在这个商品
			      for(let i=0;i<this.cardList.length;i++){
			        if(this.cardList[i].goodsId == goods.goodsId){
			          isHave = true
			        }
			      }
			      //根据isHave的值判断订单列表中是否已经有此商品
			      if(isHave){
			        //存在就进行数量增加
			        let arr = this.cardList.filter(o => o.goodsId == goods.goodsId)
			        //arr是一个新数组，arr[{goodsId:*,goodsName:*,price:*,count:*}],所以访问count需要用arr[0].count访问
			        arr[0].count++;
			      }else{
			      //不存在就推入数组
			        let newGoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1}
			        this.cardList.push(newGoods)
			      }
				  // this.getAllMoney(this.cardList)
			    },
			 delGoods(goods){
				  // this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
				  let arr = this.cardList.filter(o => o.goodsId == goods.goodsId)
				  if(arr[0].count<=1){
					this.cardList = this.cardList.filter(o => o.goodsId != goods.goodsId);
				  }else{
					arr[0].count--;
				  }
				 // this.getAllMoney(this.cardList)
			},
			
			 // getAllMoney(data){
				//   this.totalCount = 0;
				//   this.totalMoney = 0;
				//   //进行数量和价格的汇总计算
				// 	data.forEach((element) => {
				// 	  this.totalCount = this.totalCount+element.count;
				// 	  this.totalMoney = this.totalMoney + (element.price*element.count);
				// 	});
				// }
		}
	};
</script>

<style scoped>
.goods-wrapper{
	margin-top: 80px;
	z-index: 1;
}
.card-list{
	position: relative;
	overflow: hidden;
}
.card-list-item{
	margin-top: 30px;
	margin-bottom: -15px;
	height: 110px;
}
.item-middle-list{
}
.item-left{
	float: left;
	width: 100px;
	height: 110px;
	background: #a07a7a;
	line-height: 110px;
	text-align: center;
}
.item-middle{
	display: inline-block;
	width: auto;
	position: relative;
	padding-left: 15px;
	height: 110px;
}
.item-title{
	margin-bottom: 45px;
}
.item-box{
	margin-left: -10px;
}
.add-button, .reduce-button{
	display: inline-block;
	height: 15px;
	width: 30px;
}
.add-button{
	background: url(../../static/assets/group_add.png) no-repeat center;
}
.reduce-button{
	background: url(../../static/assets/group_reduce.png) no-repeat center;
}
.item-right{
	float: right;	
	width: 100px;
	position: relative;
}
.item-price{
    color: #3366ff;
}
.goods-bottom-box{
	overflow: hidden;
    border-top: 1px solid #bfb8b8;
}
.goods-bottom-box .wrapper{
	margin: 10px;
}
.goods-bottom-list{
	float: left;
}
.left-wrap .all-money{
	color: #3366FF;
}
.right-wrap{	
	float: right;
	line-height: 50px;
	height: 50px;
	margin-right: 35px;
	color: #3366FF;
	font-size: 1.5em;
}
</style>