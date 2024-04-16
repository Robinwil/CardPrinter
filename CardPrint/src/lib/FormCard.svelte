<!-- CardPrint\src\lib\FormCard.svelte -->
<script>
	export let user;
	export let printCount = 1;
</script>

<div class="form-card-container">
	{#each Array(printCount) as _}
		<div class="form-card-wrapper">
			<div class="form-card front CardGraphicFront">
				<div class="user-image" style="background-image: url({user.image});"></div>
				<h2>{user.name}</h2>
				<p>{user.department}</p>
			</div>
			<div class="form-card back CardGraphicBack">
				<h3>{user.company}</h3>
				<!-- Add more company-specific information -->
			</div>
		</div>
	{/each}
</div>

<button on:click={() => window.print()}>Print {printCount} copy/copies</button>

<style>
	.form-card-container {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.form-card-wrapper {
		page-break-after: always;
	}

	.form-card {
		position: relative;
		height: 54mm;
		width: 86mm;
		border: 1px solid #ccc;
		overflow: hidden;
		background-color: white;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 0.25in;
		box-sizing: border-box;
	}

	.user-image {
		position: absolute;
		right: 0.4cm;
		top: 0.4cm;
		width: 27mm;
		height: 31mm;
		border-radius: 1%;
		background-size: cover;
		background-position: center;
	}

	.form-card h2 {
		font-family: 'Roboto', sans-serif;
		font-weight: 500;
		position: absolute;
		left: 0.4cm;
		bottom: 1.7cm;
		font-size: 12pt;
		margin: 0;
	}

	.form-card p {
		font-family: 'Roboto', sans-serif;
		position: absolute;
		left: 0.4cm;
		bottom: 1.2cm;
		font-size: 10pt;
		margin: 0;
	}

	.form-card h3 {
		font-size: 14pt;
		margin: 0;
		margin-bottom: 0.125in;
	}

	.back {
		background-image: url(Adgangskort_Ansatt_Bakside.jpg);
		background-size: cover;
	}

	.front {
		background-image: url(Adgangskort_Ansatt_Framside.jpg);
		background-size: cover;
	}
</style>
