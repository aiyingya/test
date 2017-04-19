<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .wrap{
        max-width: 750px;
        margin: 0 auto;
        background: #fff;
        padding-bottom: 80px;
    }
    .flex-box{
        display: flex;
    }
    .flex-item{
        flex-grow: 1;
        min-width: 50%;
    }
    .price-box p{
        font-size: 12px;
        line-height: 24px;
        color: #f7b1b4;
        height: 24px;
    }
    .price-box{
        color: #fff;
        text-align: center;
    }
    .price-box>div{
        padding: 15px 0;
    }
    p.price-number{
        font-size: 20px;
        line-height: 30px;
        font-weight: bold;
        color: #fff;
    }
    .price-color-1{
        background: #ED4149;
    }
    .price-color-2{
        background: #ec565f;
    }
    .price-color-3{
        background: #ef686f;
    }
    .item-box, .car-detail{
        margin:0 4%;
        background: #fff;
    }
    .item-box{
        border-top: 1px solid #E5E5E5;
        padding: 15px 0;
        line-height: 14px;
        font-size: 14px;
        color: #666;
    }
    ul>.item-box:first-child{
        border-top: none;
    }
    .h-gary{
        padding:0 4%;
        font-size: 14px;
        color: #888888;
        line-height: 40px;
        background: #EFEFEF;
    }
    .car-detail{
        height: 60px;
        padding: 10px 0;
        border-bottom: 1px solid #E5E5E5;
    }
    .car-img{
        height: 50px;
        min-width: 50px;
        margin:5px 10px 0 0;
        float: left;
        background: #eee;
    }
    .car-img img{
        height: 100%;
    }
    .car-price{
        color: #888;
        padding-top: 5px;
    }
    .item-box-lf{
        display: inline-block;
        min-width: 40%;
        max-width: 60%;
        text-align: left;
        vertical-align: middle;
    }
    .item-box-rig{
        display: inline-block;
        min-width: 40%;
        max-width: 60%;
        text-align: right;
        vertical-align: middle;
        color: #333;
        float: right;
    }
    .item-box-rig>i{
        color: #888;
        margin-right: 5px;
    }
    .fix-box-btn{
        position: fixed;
        bottom: 0;
        left: 0;
        width: 100%;
        height: 50px;
        line-height: 50px;
        font-size: 16px;
        color: #fff;
        font-weight: 600;
        text-align: center;
        background: #ED4149;
    }
    .mark-box{
        background: #EFEFEF;
        border-radius: 5px;
        min-height: 80px;
        margin:15px 4%;
        padding: 10px;
        color: #999;
    }
    .title{
        line-height: 50px;
        font-size: 16px;
        color: #222;
        background: #fff;
        text-align: center;
    }
</style>
<template>
    <div class="wrap">
        <h4 class="title">要买车 报价单</h4>
        <div class="flex-box price-box" v-if="quotation.hasLoan">
            <div class="price-color-1 flex-item">
                <p></p>
                <p class="price-number"><em>￥</em>{{quotation.advancePayment}}</p>
                <p>首付(元)</p>
            </div>
            <div class="price-color-2 flex-item">
                <p> </p>
                <p class="price-number"><em>￥</em>{{quotation.monthlyPayment.toFixed(0)}}</p>
                <p>月供(元)</p>
            </div>
        </div>
        <div class="flex-box price-box" v-else>
            <div class="price-color-1 flex-item">s
                <p></p>
                <p class="price-number"><em>￥</em>{{quotation.totalPayment}}</p>
                <p>落地总额(元)</p>
            </div>
        </div>
        <ul v-if="quotation.hasLoan">
            <li class="item-box"><span class="item-box-lf">首付比例</span><span class="item-box-rig">{{quotation.paymentRatio}}%</span></li>
            <li class="item-box"><span class="item-box-lf">还款周期</span><span class="item-box-rig">{{quotation.stages}}年</span></li>
            <li class="item-box"><span class="item-box-lf">费率</span><span class="item-box-rig">{{quotation.expenseRate}}%</span></li>
        </ul>
        <h5 class="h-gary">基础信息</h5>
        <div  v-for="item in quotation.quotationItems">
            <div class="car-detail">
                <div class="car-img">
                    <img :src="item.itemPic" alt="">
                </div>
                <p class="ell-2">{{item.itemName}}</p>
                <p class="car-price">指导价：{{item.guidePrice/10000}}万</p>
            </div>
            <ul>
                <li class="item-box"><span class="item-box-lf">配色</span><span class="item-box-rig">{{item.specifications}}</span></li>
                <li class="item-box"><span class="item-box-lf">裸车价</span><span class="item-box-rig">
          <i v-if="item.guidePrice-item.sellingPrice!=0">(
              <img v-if="(item.guidePrice - item.sellingPrice)>=0" style="width: 8px" src="../assets/img/icon_price_down.png" alt="">
              <img v-else style="width: 8px" src="../assets/img/icon_price_up.png" alt="">
              {{Math.abs(item.guidePrice-item.sellingPrice)<100?Math.abs(item.guidePrice-item.sellingPrice)+'元':(Math.abs(item.guidePrice-item.sellingPrice)/10000).toFixed(2)+'万'}}
              /
              {{Math.abs((item.guidePrice-item.sellingPrice)/item.guidePrice*100).toFixed(0)}}点)
          </i>
          ￥{{item.sellingPrice}}</span></li>
            </ul>
        </div>
        <h5 class="h-gary">必要花费</h5>
        <ul>
            <li class="item-box"><span class="item-box-lf">购置税、上牌费、车船税、保险等</span><span class="item-box-rig">￥{{quotation.requiredExpenses}}</span></li>
        </ul>
        <h5 class="h-gary">其他花费</h5>
        <ul>
            <li class="item-box"><span class="item-box-lf">精品费、安装费等</span><span class="item-box-rig">￥{{quotation.otherExpenses}}</span></li>
        </ul>
        <h5 class="h-gary">备注</h5>
        <div class="mark-box">
            {{quotation.remark||'无备注'}}
        </div>
        <!--<a class="fix-box-btn" v-bind:href="'tel:'+quotation.customerMobile">-->
            <!--联系店长-->
        <!--</a>-->
    </div>
</template>
<script>
    module.exports = {
        data: function() {
            return {
                msg: '',
                id:this.QueryString('quotationId'),
                quotation:{},
            }
        },
        methods: {
            QueryString: function(key) {
                var uri = decodeURIComponent(window.location.href);
                var re = new RegExp("[\&\?]" + key + "\=([^\&]*)", "ig");
                return ((uri.match(re)) ? (uri.match(re)[0].substr(key.length + 2)) : null);
            }
        },
        ready: function() {
            var _this = this;
            $.get(HTTP_SERVER+'/sale/quotation/'+_this.id,function(data){
                _this.quotation = data.data;
            });
        }
    };

</script>