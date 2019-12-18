<template>
	<v-app>
		<app-menu></app-menu>
		<app-toolbar></app-toolbar>
		<v-content>
			<app-carousel></app-carousel>
		</v-content>
		<app-footer></app-footer>
	</v-app>
</template>

<script>
import Carousel from './components/Carousel.vue'
import Toolbar from './components/Toolbar.vue'
import Footer from './components/Footer.vue'
import Menu from './components/Menu.vue'

export default {
	components: {
		'app-carousel': Carousel,
		'app-toolbar': Toolbar,
		'app-footer': Footer,
		'app-menu': Menu
	}
}
</script>
