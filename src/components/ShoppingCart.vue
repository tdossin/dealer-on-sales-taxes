<template>
  <section id="cart" class="d-flex gap-5 justify-content-center">
      <div class="col-sm-4">
        <h4>Click to select your items for purchase:</h4>
        <ul class="list-group">
          <li v-for="item in taxedItems" :key="item.id" @click="addItemToCart(item)"
            class="list-group-item d-flex justify-content-between align-items-center">
            <span class="item-name">{{item.name}}
              <span v-if="item.isTaxable" class="tag taxable">t</span>
              <span v-if="item.isImported" class="tag imported">i</span>
            </span>
            <span class="price">${{parseFloat(item.price).toFixed(2)}}</span>
          </li>
        </ul>
        <div class="row">
          <div class="col-md-12 notes">
            <p><span class="tag taxable">t</span>: Taxable (+10% sales tax applies)</p>
            <p><span class="tag imported">i</span>: Imported item (+5% import tax applies)</p>
          </div>
        </div>
      </div>
      <div class="col-4">
        <h4>Purchase Summary:</h4>
        <div class="row">
          <p v-for="item in countedCart" :key="item.id">{{item.name}}:
            <span v-if="item.count <= 1">{{parseFloat(item.price + item.importTaxes +
              item.salesTaxes).toFixed(2)}}</span>
            <span v-else>
              {{parseFloat((item.price + item.importTaxes + item.salesTaxes) * item.count).toFixed(2) }} ({{item.count}}
              @ {{parseFloat(item.price + item.importTaxes + item.salesTaxes).toFixed(2) }})
            </span>
          </p>
        </div>
        <div class="row" v-if="cart.length">
          <div class="col-6">
            <p>Sales Taxes: {{parseFloat(totalSalesTaxes).toFixed(2)}}</p>
            <p>Total: {{parseFloat(totalSale).toFixed(2)}}</p>
            <button @click="emptyCart" type="button" class="btn btn-primary">Empty Cart</button>
          </div>
        </div>
      </div>
    </section>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';

type Item = {
  "name": string,
        "price": number,
        "isTaxable": boolean,
        "isImported": boolean,
        "id": string,
        "salesTaxes": number,
        "importTaxes": number,
        "count": number
}
@Options({
  data() {
    return {
      itemPriceWithTaxes: 0,
      taxedItems: [],
      cart: [],
      countedCart: [],
      totalSalesTaxes: 0,
      totalSale: 0,
    };
  },
  created() {
    this.getItemsData();
  },
  methods: {
    calcSalesTax(price:number):number {
      let addedTaxes = price * 0.1;
      let roundedTaxes = Math.ceil(addedTaxes * 20) / 20;
      return roundedTaxes;
    },
    calcImportTax(price:number):number {
      let addedTaxes = price * 0.05;
      let roundedTaxes = Math.ceil(addedTaxes * 20) / 20;
      return roundedTaxes;
    },
    getItemsData() {
      fetch("./items.json")
        .then(async (response) => response.json())
        .then(async (data) => {
          this.taxedItems = data.map((item: Item) => {
            // calculate sales tax if applicable
            if (item.isTaxable) {
              item.salesTaxes = this.calcSalesTax(item.price);
            } 
            // calculate import tax if applicable
            if (item.isImported) {
              item.importTaxes = this.calcImportTax(item.price);
            }
            return item;
          });
        });
    },
    addItemToCart(item: Item) {
      // check if item is already in cart and iterate count if so
      if (this.cart.includes(item)) {
        item.count++;
      } else {
        item.count = 1;
      }
      // add item to cart array
      this.cart.push(item);
      // remove duplicates for receipt
      this.countedCart = [...new Set(this.cart)];
      // calculate total sales taxes
      this.totalSalesTaxes =
        this.totalSalesTaxes + item.salesTaxes + item.importTaxes;
      // calculate item price with taxes for display on receipt
      this.itemPriceWithTaxes = item.price + item.salesTaxes + item.importTaxes;
      // add to total sale amount
      this.totalSale = this.totalSale + this.itemPriceWithTaxes;
    },
    emptyCart() {
      // empty cart arrays & clear totals
      this.cart.splice(0);
      this.countedCart.splice(0);
      this.totalSale = 0;
      this.totalSalesTaxes = 0;
    },
  },
  props: {
    
  }
})
export default class ShoppingCart extends Vue {
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.notes p {
  margin: 5px 0;
}
.list-group li {
  cursor: pointer;
}
.cart li {
  list-style-type: none;
}
.tag {
  margin: 2px;
  padding: 0 5px;
  font-size: 0.6em;
  font-weight: bold;
  border-radius: 30px;
}
.imported {
  background-color: #ff0000;
  color: #fff;
}
.taxable {
  background-color: #0000ff;
  color: #fff;
}
</style>
