<template>
	<ProjectDescription />
	<ShoppingCart />
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import ShoppingCart from "./components/ShoppingCart.vue";
import ProjectDescription from "./components/ProjectDescription.vue";

@Options({
	components: {
		ShoppingCart,
		ProjectDescription,
	},
})
export default class App extends Vue {}
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	color: #2c3e50;
	margin-top: 60px;
}
* {
	box-sizing: border-box;
}

html {
	font-family: "Jost", sans-serif;
}

body {
	margin: 0;
}
header {
	margin-bottom: 30px;
}
</style>
