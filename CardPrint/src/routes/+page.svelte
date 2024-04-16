<!-- CardPrint\src\routes\+page.svelte -->
<script>
	import { onMount } from 'svelte';
	import FormCard from '$lib/FormCard.svelte';
	import { users } from '$lib/db.js';

	let selectedUser = users[0];
	let printCount = 1;
	let newUser = {
		id: users.length + 1,
		name: '',
		image: '',
		department: '',
		company: 'Vestland fylkeskommune'
	};

	let companies = ['Vestland fylkeskommune', 'Other Company'];
	let selectedCompany = companies[0];

	function addUser() {
		if (newUser.name && newUser.image && newUser.department) {
			users.push({ ...newUser });
			newUser = {
				id: users.length + 1,
				name: '',
				image: '',
				department: '',
				company: selectedCompany
			};
		}
	}

	function deleteUser(userId) {
		const index = users.findIndex((user) => user.id === userId);
		if (index !== -1) {
			users.splice(index, 1);
			selectedUser = users[0];
		}
	}

	function selectUser(user) {
		selectedUser = user;
	}
</script>

<div class="app">
	<h1>Form Card Administration</h1>

	<div class="user-management">
		<h2>User Management</h2>
		<table>
			<thead>
				<tr>
					<th>Image</th>
					<th>Name</th>
					<th>Department</th>
					<th>Actions</th>
				</tr>
			</thead>
			<tbody>
				{#each users as user}
					<tr class:selected={selectedUser === user} on:click={() => selectUser(user)}>
						<td><img src={user.image} alt={user.name} width="50" /></td>
						<td>{user.name}</td>
						<td>{user.department}</td>
						<td>
							<button on:click|stopPropagation={() => deleteUser(user.id)}>Delete</button>
						</td>
					</tr>
				{/each}
			</tbody>
		</table>

		<h3>Add New User</h3>
		<form on:submit|preventDefault={addUser}>
			<select bind:value={selectedCompany}>
				{#each companies as company}
					<option value={company}>{company}</option>
				{/each}
			</select>
			<input type="text" placeholder="Name" bind:value={newUser.name} />
			{#if selectedCompany === 'Vestland fylkeskommune'}
				<select bind:value={newUser.department}>
					<option value="npi">NPI</option>
					<option value="oro">ORO</option>
					<option value="inv">INV</option>
				</select>
			{:else}
				<input type="text" placeholder="Department" bind:value={newUser.department} />
			{/if}
			<input type="text" placeholder="Image URL" bind:value={newUser.image} />
			<button type="submit">Add User</button>
		</form>
	</div>

	<div class="form-card-printing">
		<h2>Form Card Printing</h2>
		<p>Selected User: {selectedUser.name}</p>
		<input type="number" min="1" bind:value={printCount} />
		<FormCard user={selectedUser} {printCount} />
	</div>
</div>

<style>
	@import '../lib/print.css';

	.app {
		display: flex;
		justify-content: space-between;
		padding: 2rem;
	}

	.user-management {
		width: 40%;
	}

	.form-card-printing {
		width: 55%;
	}

	table {
		width: 100%;
		border-collapse: collapse;
		margin-bottom: 1rem;
	}

	th,
	td {
		padding: 0.5rem;
		text-align: left;
		border-bottom: 1px solid #ccc;
	}

	form {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}

	input,
	select {
		padding: 0.5rem;
		font-size: 1rem;
	}

	button {
		padding: 0.5rem 1rem;
		font-size: 1rem;
		background-color: #007bff;
		color: white;
		border: none;
		border-radius: 0.25rem;
		cursor: pointer;
	}

	.selected {
		background-color: #f0f0f0;
	}

	tbody tr {
		cursor: pointer;
	}

	tbody tr:hover {
		background-color: #e0e0e0;
	}
</style>
