<template>
  <div class="home">
    <div class="selection-bar">
      <div class="selection-locations">
        <label for="from">Transport from</label>
        <select class="select-default" name="from" id="from" v-model="from">
          <option value="Caerleon">Caerleon</option>
          <option value="Fort Sterling">Fort Sterling</option>
          <option value="Martlock">Martlock</option>
          <option value="Bridgewatch">Bridgewatch</option>
          <option value="Lymhurst">Lymhurst</option>
          <option value="Thetford">Thetford</option>
        </select>
        <label for="to">to</label>
        <select class="select-default" name="to" id="to" v-model="to">
          <option value="Caerleon">Caerleon</option>
          <option value="Fort Sterling">Fort Sterling</option>
          <option value="Martlock">Martlock</option>
          <option value="Bridgewatch">Bridgewatch</option>
          <option value="Lymhurst">Lymhurst</option>
          <option value="Thetford">Thetford</option>
        </select>
      </div>
      <div>
        <input class="input-default" type="text" placeholder="Capacity" v-model="capacity">
      </div>
      <div class="selection-item">
        <label for="item-select">Item</label>
        <select class="select-default margin-left" name="type-select" id="type-select" v-model="itemType">
          <option value="ORE">Ore</option>
          <option value="METALBAR">Metalbar</option>
          <option value="WOOD">Wood</option>
          <option value="PLANKS">Planks</option>
          <option value="HIDE">Hide</option>
          <option value="LEATHER">Leather</option>
          <option value="FIBER">Fiber</option>
          <option value="CLOTH">Cloth</option>
        </select>
        <select class="select-default" name="tec-select" id="tec-select" v-model="itemTec">
          <option value="T2_">T2</option>
          <option value="T3_">T3</option>
          <option value="T4_">T4</option>
          <option value="T5_">T5</option>
          <option value="T6_">T6</option>
          <option value="T7_">T7</option>
          <option value="T8_">T8</option>
        </select>
        <select class="select-default" name="ench-select" id="ench-select" v-model="itemEnch">
          <option value="">0</option>
          <option value="_LEVEL1@1">1</option>
          <option value="_LEVEL2@2">2</option>
          <option value="_LEVEL3@3">3</option>
        </select>
      </div>
      <button class="button-default" @click="getData()">Get Data</button>
    </div>

    <div class="item-list">
      <item v-bind:item="item"  v-for="item in itemList" v-bind:key="item.name"></item>
    </div>
    <div class="foot-container">
      <button class="button-default" @click="updateAll()">Update all</button>
      <button class="button-default margin-left" @click="itemList = []">Remove all</button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Item from '@/components/Item.vue'
import axios from "axios";

export default {
  name: 'home',
  components: {
    Item,
  },
  data () {
    return {
      from: "Fort Sterling",
      to: "Caerleon",
      itemType: "ORE",
      itemTec: "T4_",
      itemEnch: "",
      itemList: [],
      capacity: ""
    }
  },
  created (){
    
  },
  methods: {
    getData() {
      console.log("Getting Data...")
      if (this.from === this.to) {
        alert("From and to Location have to be different!");
        return;
      }
      let fullItem = this.itemTec + this.itemType + ((this.itemTec === "T2_" || this.itemTec === "T3_") ? "" : this.itemEnch);
      console.log(fullItem);
      let path = "https://www.albion-online-data.com/api/v1/stats/Prices/" + fullItem + "?locations=" + this.from + ", " + this.to;
      axios.get(encodeURI(path)).then(response => {
        this.createItem(response.data);
      })
    },

    updateAll () {
      if (this.itemList.length === 0) {
        return;
      }

      let itemNames = "";

      //easy way
      this.itemList.forEach(item => {
        itemNames += item.name + ",";
      });

      itemNames = itemNames.substring(0, itemNames.length - 1);

      let path = "https://www.albion-online-data.com/api/v1/stats/Prices/" + itemNames + "?locations=" + this.from + ", " + this.to;
      axios.get(encodeURI(path)).then(response => {
        let index = 0;

        for(;index < response.data.length/2; ++index) {
          let itemData = [];
          itemData.push(response.data[index * 2]);
          itemData.push(response.data[index * 2 + 1]);
          this.createItem(itemData);
        }
      })
    },

    createItem (itemdata) {
      
      let itemWeight = -1;
      switch (itemdata[0].item_id.split("_")[0]) {
        case "T2":
          itemWeight = 0.23;
          break;
        case "T3":
          itemWeight = 0.34;
          break;
        case "T4":
          itemWeight = 0.51;
          break;
        case "T5":
          itemWeight = 0.76;
          break;
        case "T6":
          itemWeight = 1.14;
          break;
        case "T7":
          itemWeight = 1.71;
          break;
        case "T8":
          itemWeight = 2.56;
          break;
      }

      let itemAmount = this.capacity / itemWeight;
      
      let item = {
        name: itemdata[0].item_id,
        weight: itemWeight,
        amount: itemAmount,
        from: {
          location: this.from,
          buy_price: -1,
          sell_price: -1
        },
        to: {
          location: this.to,
          buy_price: -1,
          sell_price: -1
        }
      }

      itemdata.forEach(location => {
        if (location.city === this.from) {
          item.from.buy_price = location.buy_price_max;
          item.from.sell_price = location.sell_price_min;
        } else {
          item.to.buy_price = location.buy_price_max;
          item.to.sell_price = location.sell_price_min;
        }
      });

      this.addItem(item);
    },

    addItem (item) {
      for (let index = 0; index < this.itemList.length; index++) {
        if (this.itemList[index].name === item.name) {

          console.log("Update Item - " + item.name);
          this.itemList[index].from = item.from;
          this.itemList[index].to = item.to;

          return;
        }
      }

      this.itemList.push(item);

    }
  }
}
</script>
<style lang="less">
@import url('../assets/less/app.less');
.home {
  max-width: 920px;
  margin: 0 auto;
}

.item-list {
  margin-top: 15px;
  display: flex;
  flex-flow: row wrap;
  align-items: stretch;
  margin-left: -10px;
}

.selection-bar {
  border-bottom: 1px solid #cdcdcd;
  display: flex;
  padding: 5px 10px;
  justify-content: space-between;
  align-items: center;

  >div {
    flex-grow: 1;
  }

  .selection-locations {
    flex-grow: 2;
    display: flex;
    align-items: center;

    >* {
      margin-right: 5px;
    }
  }

  .selection-item {
    flex-grow: 2;
  }
}

.margin-left {
  margin-left: 15px;
}

.foot-container {
  margin-top: 15px;
}

.button-default {
  background-color: @primary-blue;
  padding: 7px 10px;
  border: none;
  color: #efefef;
}

.input-default {
  padding: 6px 10px;
  border: 1px solid #e1e1e1;
  outline: none;
}

.select-default {
  padding: 5px 10px;
  border: 1px solid #e1e1e1;
}
</style>