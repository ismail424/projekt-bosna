<script lang="ts">
	import { Router, Route } from "svelte-routing";
	import Home from "./routes/Home.svelte";
	import Info from "./routes/Info.svelte";
	import NotFound from "./routes/NotFound.svelte";
	import RealTime from "./routes/RealTime.svelte";
	import { locale } from './i18n';

	const changeLanguage = (e) => {
		console.log($locale);
		if ($locale === 'sv') {
			document.getElementById('change_language').innerHTML = '🇸🇪 Svenska';
			$locale = 'ba';
		} else {
			$locale = 'sv';
			document.getElementById('change_language').innerHTML = '🇧🇦 Bosanski';

		}

		
	};
</script>

<Router>
	<div>
	  <Route path="info" component="{Info}" />
	  <Route path="/"><Home /></Route>
	  <Route path="*" component="{NotFound}" />
	  <Route path="/realtime" component="{RealTime}" />
	</div>
</Router>
<button id="change_language" on:click="{changeLanguage}">🇸🇪 Svenska</button>


<style lang="scss">
 #change_language{
	 position: fixed;
	 top: 2rem;
	 right: 2rem;
	 font-size: larger;
	 color: #333;
	 font-weight: 900;
	 background-color: rgb(255, 253, 253);
	 border-radius: 1rem;
	 padding: 1rem;
	 &:hover{
		cursor: pointer;
		opacity: 0.6;
	 }

	 @media screen and (max-width: 600px) {
		 top: 1rem;
		 right: 1rem;
		 padding: 0.5rem;
		 font-size: medium;
	 }
		
 }
</style>

