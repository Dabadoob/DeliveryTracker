<script>
	import seedData from './seed-data.js'
	const entriesKey = 'entries'
	let input = getDefaultInput()
	let entries
	let averageMilesPerHour

	const savedData = localStorage.getItem(entriesKey)
	console.log(seedData)
	setEntries(savedData || seedData) // if we've saved data to localstorage, use that, else use the seed data for now (just proof-of-concept)

	function submit(e) {
		e.preventDefault()
		setEntries(entries.concat(input))
		localStorage.setItem(entriesKey, JSON.stringify(entries))
		input = getDefaultInput()
	}

	function getDefaultInput() {
		return {
			date: new Date().toISOString().substring(0,10),
			hoursWorked: 0,
			deliveriesCompleted: 0,
			commissionEarned: 0,
			tipsEarned: 0,
			cashEarned: 0,
			boost: 0,
			bonus: 0,
			endOfWeekSubsidyAdded: 0,
			other: 0,
			milesDriven: 0,
		}
	}

	function setEntries(val) {
		entries = val
		averageMilesPerHour = sum(e => e.milesDriven) / sum(e => e.hoursWorked)
	}


	function sum(valueSelector) {
		return entries.filter(e => valueSelector(e) != null).map(e => valueSelector(e)).reduce((sum, next) => sum + next, 0);
	}
</script>

<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
<style>
	.main {
		font-family: arial;
		display: flex;
	}

	.main > div {
		margin: 20px;
		border-radius: 10px;
		padding: 20px;
	}

	table {
		overflow-x: auto;
	}

	form > div {
		margin-bottom: 20px;
	}

	form > div > label {
		display: block;
	}
</style>

<div class="main">
	<div>
		<h2>Enter day's work</h2>
		<form on:submit={submit}>
			<div>
				<label>Date</label>
				<input type="date" bind:value={input.date} />
			</div>
			<div>
				<label>Hours worked</label>
				<input type="number" bind:value={input.hoursWorked} />
			</div>
			<div>
				<label>Deliveries completed</label>
				<input type="number" bind:value={input.deliveriesCompleted} />
			</div>
			<div>
				<label>Commission earned</label>
				<input type="number" bind:value={input.commissionEarned} />
			</div>
			<div>
				<label>Tips earned</label>
				<input type="number" bind:value={input.tipsEarned} />
			</div>
			<div>
				<label>Cash earned</label>
				<input type="number" bind:value={input.cashEarned} />
			</div>
			<div>
				<label>Boost</label>
				<input type="number" bind:value={input.boost} />
			</div>
			<div>
				<label>Bonus</label>
				<input type="number" bind:value={input.bonus} />
			</div>
			<div>
				<label>End of week subsidy added</label>
				<input type="number" bind:value={input.endOfWeekSubsidyAdded} />
			</div>
			<div>
				<label>Other</label>
				<input type="number" bind:value={input.other} />
			</div>
			<div>
				<label>Miles driven</label>
				<input type="number" bind:value={input.milesDriven} />
			</div>
			<button type="submit">Submit</button>
		</form>
	</div>

	<div>
		<div>
			<h2>Dashboard</h2>
			<p>averageMilesPerHour: {averageMilesPerHour}</p>
			<p>Average hourly ytd</p>
			<p>Average hourly last 3 months</p>
			<p>etc. and ability to make custom reports...</p>
		</div>
		<div>
			<h2>Days entered</h2>
			<table class="table table-bordered">
				<thead>
					<tr>
						<th>Date</th>
						<th>Hours worked</th>
						<th>Deliveries completed</th>
						<th>Commission earned</th>
						<th>Tips earned</th>
						<th>Cash earned</th>
						<th>Boost</th>
						<th>Bonus</th>
						<th>End of week subsidy added</th>
						<th>Other</th>
						<th>Miles driven</th>
					</tr>
				</thead>
				<tbody>
					{#each entries as entry}
						<tr>
							<td>{entry.date || ''}</td>
							<td>{entry.hoursWorked || ''}</td>
							<td>{entry.deliveriesCompleted || ''}</td>
							<td>{entry.commissionEarned || ''}</td>
							<td>{entry.tipsEarned || ''}</td>
							<td>{entry.cashEarned || ''}</td>
							<td>{entry.boost || ''}</td>
							<td>{entry.bonus || ''}</td>
							<td>{entry.endOfWeekSubsidyAdded || ''}</td>
							<td>{entry.other || ''}</td>
							<td>{entry.milesDriven || ''}</td>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
</div>
</div>