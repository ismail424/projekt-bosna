<script lang="ts">
	import { Router, Route } from "svelte-routing";
	import Home from "./routes/Home.svelte";
	import Info from "./routes/Info.svelte";
	import NotFound from "./routes/NotFound.svelte";
</script>

<Router>
	<div>
	  <Route path="info" component="{Info}" />
	  <Route path="/"><Home /></Route>
	  <Route path="*" component="{NotFound}" />
	</div>
</Router>


