<template>
    <div class="item">
        <div class="refresh-container">
            <button @click="refresh()">
                <img src="../assets/images/refresh.svg" alt="Refresh Item">
            </button>
        </div>
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
                        Profit (incl. Taxes)
                    </h4>
                </div>
                <div class="item-location-info-prices">
                    <div>
                        <p>Buy to Buy: {{buybuy - buybuytaxes}}</p>
                        <p>Buy to Sell: {{buysell - buyselltaxes}}</p>
                    </div>
                    <div>
                        <p>Sell to Buy: {{sellbuy - sellbuytaxes}}</p>
                        <p>Sell to Sell: {{sellsell - sellselltaxes}}</p>
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
    methods: {
        refresh () {
            this.$emit('update');
        }
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
        buybuytaxes: function () {
            return Math.ceil(this.item.to.buy_price * 0.03 + this.item.from.buy_price * 0.015); 
        },
        sellbuytaxes: function () {
            return Math.ceil(this.item.to.buy_price * 0.03);
        },
        buyselltaxes: function () {
            return Math.ceil(this.item.to.sell_price * 0.045 + this.item.from.buy_price * 0.015);
        },
        sellselltaxes: function () {
            return Math.ceil(this.item.to.sell_price * 0.045);
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
    position: relative;
}

.refresh-container {
    position: absolute;
    top: 10px;
    right: 10px;

    button {
        background: none;
        border: none;
        cursor: pointer;
        outline: none;
        padding: 5px 5px 3px 5px;
        
        &:hover {
            background: lighten(#efefef, 10%);
            border: 1px solid #cdcdcd;
            border-radius: 3px;
        }
    }
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