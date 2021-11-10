<template>
	<section class="row d-flex gap-5 justify-content-center">
		<div class="col-md-9">
			<h2>
				Problem
				<button
					@click="toggleProblem"
					type="button"
					class="btn btn-sm btn-secondary"
				>
					<span v-if="showDescription">Hide</span> <span v-else>Show</span>
				</button>
			</h2>
		</div>
	</section>
	<section class="d-flex gap-5 justify-content-center" v-if="showDescription">
		<div class="col-sm-8">
			<p>
				There are a variety of items for sale at a store. When a customer
				purchases items, they receive a receipt. The receipt lists all of the
				items purchased, the sales price of each item (with taxes included), the
				total sales taxes for all items, and the total sales price.
			</p>
			<p>
				Basic sales tax applies to all items at a rate of 10% of the item’s list
				price, with the exception of books, food, and medical products, which
				are exempt from basic sales tax. An import duty (import tax) applies to
				all imported items at a rate of 5% of the shelf price, with no
				exceptions.
			</p>
			<p>
				Write an application that takes input for shopping baskets and returns
				receipts in the format shown below, calculating all taxes and totals
				correctly. When calculating the sales tax, round the value up to the
				nearest 5 cents. For example, if a taxable item costs $5.60, an exact
				10% tax would be $0.56, and the final price after adding the rounded tax
				of $0.60 should be $6.20.
			</p>

			<div class="row">
				<div class="col-sm-6">
					<strong>INPUT 1</strong><br />
					1 Book at 12.49<br />
					1 Book at 12.49<br />
					1 Music CD at 14.99<br />
					1 Chocolate bar at 0.85
				</div>
				<div class="col-sm-6">
					<strong>OUTPUT 1</strong><br />
					Book: 24.98 (2 @ 12.49)<br />
					Music CD: 16.49<br />
					Chocolate bar: 0.85<br />
					Sales Taxes: 1.50<br />
					Total: 42.32
				</div>
			</div>
			<div class="row">
				<div class="col-sm-6">
					<strong>INPUT 2</strong><br />
					1 Imported box of chocolates at 10.00<br />
					1 Imported bottle of perfume at 47.50
				</div>
				<div class="col-sm-6">
					<strong>OUTPUT 2</strong><br />
					Imported box of chocolates: 10.50<br />
					Imported bottle of perfume: 54.65<br />
					Sales Taxes: 7.65<br />
					Total: 65.15
				</div>
			</div>
			<div class="row">
				<div class="col-sm-6">
					<strong>INPUT 3</strong><br />
					1 Imported bottle of perfume at 27.99<br />
					1 Bottle of perfume at 18.99<br />
					1 Packet of headache pills at 9.75<br />
					1 Imported box of chocolates at 11.25<br />
					1 Imported box of chocolates at 11.25
				</div>
				<div class="col-sm-6">
					<strong>OUTPUT 3</strong><br />
					Imported bottle of perfume: 32.19<br />
					Bottle of perfume: 20.89<br />
					Packet of headache pills: 9.75<br />
					Imported box of chocolates: 23.70 (2 @ 11.85)<br />
					Sales Taxes: 7.30<br />
					Total: 86.53
				</div>
			</div>
		</div>
	</section>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";

@Options({
	data() {
		return {
			showDescription: true,
		};
	},
	methods: {
		toggleProblem() {
			this.showDescription = !this.showDescription;
		},
	},
	props: {},
})
export default class ProjectDescription extends Vue {}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.notes p {
	margin: 5px 0;
}
h2 span {
	font-size: 1em;
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
