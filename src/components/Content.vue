
<template>
	<div class="portfolio-box">
		<!-- Left Navigation -->
		<div class="portfolio-nav">
			<button type="button" class="btn btn-dark" data-bs-target="#carouselExampleControls" @click="goToSlide(0)">Projects</button>

			<button type="button" class="btn btn-dark" data-bs-target="#carouselExampleControls" @click="goToSlide(1)">Tools</button>
		</div>

		<!-- Carousel Content -->
		<div id="carouselExampleControls" ref="carouselElement" class="carousel slide carousel-box">

			<div class="carousel-inner">

				<!-- Projects -->
				<div class="carousel-item active">
					<h2 class="text-center mb-4">Projects</h2>

					<div class="container-fluid d-flex flex-column"
					id="projects-items">

					<a href="#project-item1">
						E-commerce API Documentation.
					</a>

					<a href="#project-item2">
						Course Booking API Documentation.
					</a>

					<a href="#project-item3">
						Course Booking App.
					</a>

					<a href="#project-item4">
						E-commerce App.
					</a>

					<a href="#project-item5">
						Airline Booking System Mockup.
					</a>

					<a href="#project-item6">
						Airline Booking System Prototype.
					</a>

					<a href="#project-item7">
						Short Courses Capstone.
					</a>

				</div>
			</div>

			<!-- Tools -->
			<div class="carousel-item">
				<h2 class="text-center mb-4">Tools</h2>

				<div class="container-fluid d-flex justify-content-around">

					<div class="d-flex flex-column">
						<h3>Frontend</h3>
						<a href="#frontend">HTML 5</a>
						<a href="#frontend">CSS</a>
						<a href="#frontend">Bootstrap</a>
						<a href="#frontend">JavaScript</a>
					</div>

					<div class="d-flex flex-column">
						<h3>Backend</h3>
						<a href="#backend">Nodejs</a>
						<a href="#backend">MongoDB</a>
						<a href="#backend">Express</a>
						<a href="#backend">Postman</a>
					</div>

					<div class="d-flex flex-column">
						<h3>Other Tools</h3>
						<a href="#other-tools">HTML 5</a>
						<a href="#other-tools">CSS</a>
						<a href="#other-tools">Bootstrap</a>
						<a href="#other-tools">JavaScript</a>
					</div>
				</div>
			</div>
		</div>
	</div>
</div>
</template>


<script setup>
	import { ref, onMounted } from 'vue';
	import Carousel from 'bootstrap/js/dist/carousel';

	const carouselElement = ref(null);
	let carouselInstance = null;

	onMounted(() => {
		carouselInstance = new Carousel(carouselElement.value, {
			interval: false,
			ride: false,
			wrap: true
		});
	});

	function goToSlide(index) {
		if (carouselInstance) {
			carouselInstance.to(index);
		}
	}
</script>