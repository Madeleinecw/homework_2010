<template lang="html">
	<div id="sightingsGrid">
		<sighting v-for="sighting in sightings" :sighting="sighting" />
	</div>
</template>

<script>
import Sighting from './Sighting';
import SightingService from '../services/SightingService.js'
import {eventBus} from '../main.js'


export default {
	name: 'sightings-grid',
	data() {
		return{
		sightings: [] 
		}
	},
	mounted(){
	SightingService.getSightings()
    .then(sightings => this.sightings = sightings);

	eventBus.$on('bird-added', (bird) => {
      this.sightings.push(bird)
	})
	eventBus.$on('bird-deleted', (id) => {
      let index = this.sightings.findIndex(bird => bird._id === id)
      this.sightings.splice(index, 1)
    })

	},
	components: {
		'sighting': Sighting
	}
}
</script>

<style lang="css" scoped>
#sightingsGrid {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-evenly;
}

h2 {
	padding: 0;
	margin: 0;
}
</style>
