
<script setup>

	import {ref,onMounted,onBeforeUnmount} from "vue";
	import {Notyf} from "notyf";
	import axios from "axios"
	import 'notyf/notyf.min.css';
	const notyf = new Notyf();

	const WEB3FORMS_ACCESS_KEY= "b7e31c44-bf6c-4744-a474-e086e2c30e50";

	const SITE_KEY="6LfNxxouAAAAAOWiDw6vy6AnVc9r6L8MCLD1dSMj";

	// Initial value for the email:
	const subject = "New message from Portfolio Contact Form";

	const name = ref("");
	const email = ref("");
	const message=ref ("");

	const isLoading = ref(false);

	const submitForm =async()=>{
		if (!recaptchaToken.value) {
			notyf.error('Please verify that you are not a robot.');
			return;
		}

		isLoading.value=true;


		try{
			const response = await axios.post("https://api.web3forms.com/submit",{
				access_key : WEB3FORMS_ACCESS_KEY,
				subject: subject,
				name: name.value,
				email: email.value,
				message: message.value
			},{
				headers:{
					'Content-Type': "application/json",
					Accept: "application/json"
				}
			}
		)
			const result = response.data
			if(result.success){
				notyf.success("Message sent!")
				name.value="";
				email.value ="";
				message.value="";
			}

		}catch(e){
			notyf.error("Failed to send email");
		}finally{
			isLoading.value= false;
		}
	}
	const recaptchaContainer = ref(null);
	const recaptchaWidgetId = ref(null);
	const recaptchaToken = ref('');

	// Callback called by reCAPTCHA when successful
	function onRecaptchaSuccess(token) {
		recaptchaToken.value = token;
	}

	// Callback when expired
	function onRecaptchaExpired() {
		recaptchaToken.value = '';
	}

	// Function to render the reCAPTCHA widget
	function renderRecaptcha() {
		if (!window.grecaptcha) {
			console.error('reCAPTCHA not loaded');
			return;
		}

		recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
			sitekey: SITE_KEY,
			size: 'normal', // or 'compact'
			callback: onRecaptchaSuccess,
			'expired-callback': onRecaptchaExpired,
		});
	}

	// Function to reset reCAPTCHA 
	function resetRecaptcha() {
		if (recaptchaWidgetId.value !== null) {
			window.grecaptcha.reset(recaptchaWidgetId.value);
			recaptchaToken.value = '';
		}
	}



	onMounted(() => {
	// This code waits for the Google reCAPTCHA library to load, then renders the reCAPTCHA widget using onMounted hook. 
	// The widget is rendered with grecaptcha.render(), which requires a sitekey. 
	// Callback functions handle success and expiration events. 
	// reCAPTCHA is reset upon form submission to clear the token.
		const interval = setInterval(() => {
			if (window.grecaptcha && window.grecaptcha.render) {
				renderRecaptcha();
				clearInterval(interval);
			}
		}, 100);

		onBeforeUnmount(() => {
			clearInterval(interval);
		});
	});

</script>
<template>
	<div class="container-fluid my-5" id="contact">
		<h1>Contact</h1>

		<div class="d-flex flex-column flex-sm-row my-2">
			<div class="d-flex mx-auto">
				<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d1004778.3042661361!2d123.02251544745312!3d10.348661465670128!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x33a973be8037a443%3A0x840b848d4ec2c6c4!2sCebu!5e0!3m2!1sen!2sph!4v1791169749742!5m2!1sen!2sph" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="strict-origin-when-cross-origin"></iframe>
			</div>


			<div class="flex-folumn gap-2 col-7 mx-auto" >
				<form @submit.prevent = "submitForm" class="text-center mx-auto">
					<div class="form-floating mb-3 w-100">
						<input type="text" class="form-control" id="username" placeholder="First Name M.I. Name">
						<label for="username">Input name</label>
					</div>

					<div class="form-floating mb-3 w-100">
						<input type="email" class="form-control" id="user-email" placeholder="name@example.com">
						<label for="user-email">Email</label>
					</div>

					<div class="form-floating mb-3 w-100">
						<textarea class="form-control" id="user-textarea" placeholder="Message" ></textarea>
						<label for="user-textarea">Message</label>
					</div>

					<button type="submit" class="btn btn-primary">Submit</button>

					<div class="d-flex justify-content-end mt-2">
						<div ref="recaptchaContainer"></div>
					</div>
				</form>

			</div>
		</div>
	</div>
	
</template>