<template>
	<div id="app">
		<div class="container">
			<app-header></app-header>
			<app-directory></app-directory>
		</div>
	</div>
</template>

<script>
import Header from './components/Header.vue';
import Directory from './components/Directory.vue';
export default {
	name: 'App',
	components: {
		appDirectory: Directory,
		appHeader: Header,
	},
};
</script>

<style lang="scss">

:root {
	--color-primary: #00a896;
	--color-secondary: #fc0;
	--color-tertiary: #f0f3bd;
	--color-blue: #05668d;
	--color-dave: #f0f3bd;
}

*,
*::before,
*::after {
	margin: 0;
	padding: 0;
	box-sizing: inherit;
}

html {
	box-sizing: border-box;
	font-size: 56.25%;

}

#app {
	padding: 1rem;
	background: linear-gradient(to right bottom, var(--color-primary), var(--color-secondary));
  height: 100%;
  min-height: 100vh;
	font-family: 'Roboto', sans-serif;
}

.container {
  background: var(--color-tertiary);
  width: 99%;

  padding: 1.5rem;
  border-radius: 4px;

}



@media only screen and (min-width: 37.5em) {
  html {
    font-size: 62.5%;
  }

  .container {
    max-width: 70%;
    margin: 2rem auto;
  }
}


</style>
