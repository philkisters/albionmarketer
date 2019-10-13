<template>
    <div class="item">
        <div class="item-name-container">
            <h3>{{item.name}}</h3>
        </div>
        <div class="item-info-container">

            <div class="item-info">
                <p v-if="item.amount != 0">
                    Movable Items: {{Math.floor(item.amount)}}
                </p>
                <p v-else>
                    Item weight: {{item.weight}}
                </p>
            </div>
            <div class="item-location-info">
                <div>
                    <h4>Prices</h4>
                </div>
                <div class="item-location-info-prices">
                    <div>
                        <h5>{{item.from.location}}</h5>
                        <p>
                            Buy Price: {{item.from.buy_price}}
                        </p>
                        <p>
                            Sell Price: {{item.from.sell_price}}
                        </p>
                    </div>
                    <div>
                        <h5>
                            {{item.to.location}}
                        </h5>
                        <p>
                            Buy Price: {{item.to.buy_price}}
                        </p>
                        <p>
                            Sell Price: {{item.to.sell_price}}
                        </p>
                    </div>
                </div>
            </div>
            <div class="item-location-info">
                <div>
                    <h4>
                        Revenue
                    </h4>
                </div>
                <div class="item-location-info-prices">
                    <div>
                        <p>BB: {{buybuy}}</p>
                        <p>BS: {{buysell}}</p>
                    </div>
                    <div>
                        <p>SB: {{sellbuy}}</p>
                        <p>SS: {{sellsell}}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        item: Object
    },
    computed: {
        buybuy: function () {
            return this.item.to.buy_price - this.item.from.buy_price; 
        },
        sellbuy: function () {
            return this.item.to.buy_price - this.item.from.sell_price;
        },
        buysell: function () {
            return this.item.to.sell_price - this.item.from.buy_price;
        },
        sellsell: function () {
            return this.item.to.sell_price - this.item.from.sell_price;
        },
    }
}
</script>
<style lang="less">
.item {
    border: 1px solid #cdcdcd;
    padding: 15px;
    border-radius: 3px;
    background: #efefef;
    display: flex;
    flex-flow: column nowrap;
    margin-left: 10px;
    margin-top: 10px;
    max-width: 268px;
    width: 100%;
}

.item-name-container {
    border-bottom: 1px solid #cdcdcd;
}

.item-info-container {
    display: flex;
    flex-flow: column;

    >div {
        margin: 5px 0;
    }

    h4, h5, p {
        margin: 0 0 5px 0;
    }
}

.item-location-info-prices {
    display: flex;
    flex-flow: row nowrap;
    align-items: stretch;

    >div {
        border-radius: 4px;
        background: #fdfdfd;
        padding: 5px;
        display: flex;
        flex-flow: column nowrap;
        flex-grow: 1;
        
        &:last-child {
            margin-left: 5px;
        }
    }
}
</style>