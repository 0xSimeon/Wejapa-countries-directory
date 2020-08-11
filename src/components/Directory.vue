<template>
	<div>
		<p>Here, you can find countries, the states available and the cities in those states.</p>
		<p>How does it <span class="select">work?</span></p>
		<p>Simply select a country below 👇</p>
		<hr />
		<section class="directory__grid">
			<form action="">
				<label for="countries"
					>Select a country:
					<select name="countries" id="countries" @change="getCountries($event)" v-model="selectedCountry">
						<option :value="country.name" v-for="(country, index) in countries" :key="index">
							{{ country.name }}
						</option>
					</select>
				</label>
				<label for="states">Select a state</label>
				<select name="states" id="states" @change="getStates($event)" v-model="selectedState">
					<option :value="state.name" type="text" v-for="(state, index) in states" :key="index">
						{{ state.name }}
					</option>
				</select>
				<slide-x-Left-transition>
					<p v-if="noStateError" class="error">
						Sorry, the country you selected has no state. select another country.
					</p>
				</slide-x-Left-transition>
				<label for="cities"> Select a city: </label>
				<select name="cities" id="cities" v-model="selectedCity">
					<option :value="city.name" v-for="(city, index) in cities" :key="index"> {{ city.name }} </option>
				</select>
				<slide-x-Left-transition>
					<p v-if="noCityError" class="error">
						Sorry, the state you selected has no city. select another state.
					</p>
				</slide-x-Left-transition>
			</form>

			<div>
				<hr v-if="mobile" />
				<slide-y-down-transition>
					<p v-if="selectedCountry">
						Selected Country: <span class="selected">{{ selectedCountry }}</span>
					</p>
				</slide-y-down-transition>
				<slide-y-down-transition>
					<p v-if="selectedState">
						Selected State: <span class="selected">{{ selectedState }}</span>
					</p>
				</slide-y-down-transition>

				<slide-y-down-transition>
					<p v-if="selectedCity">
						Selected city: <span class="selected">{{ selectedCity }}</span>
					</p>
				</slide-y-down-transition>
			</div>
		</section>
	</div>
</template>

<script>
import axios from 'axios';
import { SlideXLeftTransition, SlideYDownTransition } from 'vue2-transitions';
export default {
	data() {
		return {
			countries: [],
			states: [],
			cities: [],
			noStateError: false,
			noCityError: false,
			selectedCountry: '',
			selectedState: '',
			selectedCity: '',
			mobile: false,
		};
	},
	components: {
		SlideXLeftTransition,
		SlideYDownTransition,
	},
	created() {
		// Get the countries on page-load.
		axios
			.get(
				'https://raw.githubusercontent.com/dr5hn/countries-states-cities-database/master/countries%2Bstates%2Bcities.json'
			)
			.then((response) => {
				// limit the number of countries to just 100.
				this.countries = response.data;
			});
	},
	mounted() {
		// small change on mobile only
		if (window.innerWidth < 500) return (this.mobile = true);
	},
	methods: {

		getCountries(event) {
			const currentElement = event.target.value;
			// Find the index of the option the user selected.
			const index = this.countries.findIndex((el) => el.name === currentElement);
			const states = this.countries[index].states;

			if (states.length) {
				this.states = states;
			} else {
				// if no state, show an error and remove it after 2 seconds.
				this.noStateError = !this.noStateError;
				this.states = [];
				setTimeout(() => {
					this.noStateError = false;
				}, 2000);
			}
		},
		// Similar logic
		getStates(event) {
			const currentElement = event.target.value;
			const index = this.states.findIndex((el) => el.name === currentElement);
			const cities = this.states[index].cities;

			if (cities.length) {
				this.cities = cities;
			} else {
				this.noCityError = !this.noCityError;
				this.cities = [];

				setTimeout(() => {
					this.noCityError = false;
				}, 2000);
			}
		},
	},
};
</script>

<style lang="scss" scoped>
p {
	margin: 1rem auto;
}

p, select, label {
	font-size: 2rem;
}

form, select, p, .error  {
	padding: 1rem;
}

select {
	width: 100%;
	border-radius: 4px;
	box-shadow: 0 1px 0 1px rgba(0, 0, 0, 0.04);
	margin-bottom: 1rem;
	margin-top: 1rem;
	font-family: inherit;
}

select:focus {
	border-color: #aaa;
	box-shadow: 0 0 1px 3px rgba(59, 153, 252, 0.7);
	color: #222;
	outline: none;
}

label {
	display: block;
}

.select {
	border-bottom: 3px solid var(--color-primary);
}

hr {
	border-top: 0;
	border-bottom: 3px dashed var(--color-primary);
	margin: 1rem auto;
}

.selected {
	color: var(--color-tertiary);
	background: var(--color-blue);
	padding: 0.5rem;
	border-radius: 4px;
}

.error {
	color: var(--color-tertiary);
	background: palevioletred;
	border-radius: 4px;
}

.directory {
	&__grid {
		display: flex;
		flex-direction: column;
	}
}

@media only screen and (min-width: 37.5em) {
	.directory {
		&__grid {
			display: grid;
			grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
			grid-template-rows: max-content;
		}
	}
}
</style>
