<script>
	import { onMount } from 'svelte';
	import Weather from '../components/Weather.svelte';


	let longitude;
	let latitude;
	let locationSupported;
	let dataLoading = true;
	let defaultLocation = false;
	let weather;
	let days = Array();
	let mini='mini';
	let apiUri;
	export let currentCondition = "clear-day";

	$: currentCondition;
	$: weather;
	$: dataLoading;
	$: days;
	
	function getWeather(longitude, latitude) {
		// WEATHER API GOES HERE
		apiUri = `https://......./api/weather?longitude=${longitude}&latitude=${latitude}`
		fetch(apiUri)
		  .then(r => r.json())
		  .then(function(data){
			  weather = data
			  currentCondition = weather.currently.icon
			  getDays(weather.daily)
			//   console.log(weather)
			  toggleLoading()
		  })
	}

	function getDays(daily) {
		var data = daily.data.slice(0,5)

		data.forEach(element => {
			var a = new Date(element.time*1000)
			var dayStrings = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];
			days.push(dayStrings[a.getDay()])
		});
	}

	function getLocation() {
		if (navigator.geolocation) {
			dataLoading = true
			locationSupported = true
			navigator.geolocation.getCurrentPosition(setLocation, locationError)
		} else {
    		locationSupported = false
			noLocation()
		}
	}
	
	function setLocation(position) {
		longitude = position.coords.longitude
		latitude = position.coords.latitude
		getWeather(longitude, latitude)
	}
	
	function locationError(err) {
		noLocation()
	}
		
	function noLocation() {
		longitude = 28.283333
		latitude = -15.416667
		toggleLoading()
	}

	function toggleLoading() {
		dataLoading = !dataLoading
	}

	function toggleDefault() {
		defaultLocation = !defaultLocation
	}

	onMount(getLocation);
</script>

<svelte:head>
	{#if weather != undefined}
		<title>Weather | {weather.timezone}</title>
	{:else}
		<title>Weather</title>
	{/if}

</svelte:head>

<main>
	{#if !dataLoading}
		{#if weather != undefined}

			<div class="uk-container uk-container-xsmall">
				<div class="uk-margin-small-top">
					<h1 class="uk-text-center">Local Weather</h1>
					<ul class="uk-flex-center" uk-tab>
						<li class="uk-active"><a href="#">now</a></li>
						<li><a href="#">forecast</a></li>
					</ul>
					<ul class="uk-switcher uk-margin-small-top">
						<li>
							<div class="uk-grid-small uk-child-width-expand@s uk-text-center" uk-grid>
								<div class="uk-margin-medium-top">
									<Weather condition={currentCondition} />
									<div class="uk-text-lead">{weather.currently.summary}</div>
									<!-- <div>{weather.daily.summary}</div> -->
									<div class="uk-text-meta">Time Zone: {weather.timezone}</div>
									<div class="uk-heading-xlarge  uk-margin-remove-bottom">{weather.currently.temperature.toFixed(0)}&#8451;</div>
									<div class="uk-text-meta uk-text-uppercase">current temperature</div>
								</div>
							</div>
							<div class="uk-grid-large uk-margin-small-top" uk-grid>
								<div class="uk-width-1-2 uk-text-right">
									<div class="uk-heading-small uk-margin-remove-bottom">{weather.daily.data[0].temperatureMin.toFixed(0)}&#8451;</div>
									<div class="uk-text-meta uk-padding uk-padding-remove-vertical uk-text-uppercase">low</div>
								</div>
								<div class="uk-width-1-2 uk-text-left">
									<div class="uk-heading-small uk-margin-remove-bottom">{weather.daily.data[0].temperatureMax.toFixed(0)}&#8451;</div>
									<div class="uk-text-meta uk-padding uk-padding-remove-vertical uk-text-uppercase">high</div>
								</div>
							</div>                        
						</li>

						<li>
							<div class="uk-grid-small  uk-child-width-1-1 uk-text-center slim" uk-grid>
								<Weather condition={weather.daily.icon} />
								<div class="uk-text-lead uk-padding-remove">{weather.daily.summary}</div>
								<ul uk-accordion class="accordion uk-text-left">
									
								{#each days as dayOfWeek, i}
									<li>
										<a class="uk-accordion-title" href="#">
										<div class="uk-grid-collapse" uk-grid>
											<div class="uk-width-expand">
												<div>{dayOfWeek}</div>
											</div>
											<div class="uk-width-1-5">
												<div><Weather mini={mini} condition={weather.daily.data[i].icon} /></div>
											</div>
											<div class="uk-width-1-4">
												<div>{weather.daily.data[i].temperatureMax.toFixed(0)}&#8451;</div>
											</div>
											<div class="uk-width-1-5 text-deemphasize">
												<div>{weather.daily.data[i].temperatureMin.toFixed(0)}&#8451;</div>
											</div>
										</div>
										</a>
										<div class="uk-accordion-content uk-text-center">
											<p>Expected conditions:
											<br>{weather.daily.data[i].summary}</p>
										</div>
									</li>
								{/each}

								</ul>
							</div>
						</li>
					</ul>
				</div>
			</div>
		{/if}
	{:else}
		<p class="uk-text-center uk-margin-large-top"><span class="uk-margin-large-top" uk-spinner="ratio: 5"></span></p>
	{/if}
	
</main>

<style>
	.text-deemphasize{
		font-size: 80%;
		color: rgb(0, 101, 126);
	}
	.slim{
		max-width: 30em;
		margin: 0 auto;
	}
	.accordion li{
		margin-top: 10px;
	}
	.uk-accordion-content{
		margin: 0 0 10px;
	}
</style>