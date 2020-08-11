<template>
	<div class="header">
		<h1>
			<a href="#" class="header__link"
				><img class="header__logo" src="../assets/wejapa.png" alt="Wejapa logo" loading="lazy" /></a
			> <span class="header__text">Countries</span>
		</h1>
	</div>
</template>

<script>
export default {};
</script>

<style lang="scss">
h1 {
	font-size: 3.5rem;
	margin-bottom: 1rem;
	text-align: center;
	font-family: 'Merriweather', serif;
	display: flex;
	align-items: center;
	justify-content: center;
}

.header {
	&__link {
		text-decoration: none;
	}

	&__logo {
		width: 50px;
		height: 50px;
	}
	&__text {
		background: linear-gradient(var(--color-primary), var(--color-secondary));
		background-clip: text;
		-webkit-text-fill-color: transparent;
	}
}
</style>
