<script>
	import { v4 as uuidv4 } from 'uuid';

	let data = [
		{
			text: 'Write my first post',
			uid: 'a5bbaaba-7c00-46dc-ad5a-f13bf035922d',
			hows: ['a5bbaaba-7c00-11dc-ad5a-f13bf035922d', '1011aaba-7c00-01dc-115a-f13bf035922d'],
			whys: ['a5bbaaba-7c00-01dc-ad5a-f13bf035922d']
		},
		{ text: 'Upload the post to the blog', uid: 'a5bbaaba-7c00-11dc-ad5a-f13bf035922d', hows: [] },
		{ text: 'Publish the post at Facebook', uid: 'a5bbaaba-7c00-01dc-ad5a-f13bf035922d' },
		{
			text: 'Build a Van',
			uid: '1011aaba-7c00-01dc-115a-f13bf035922d',
			hows: ['a5bbaaba-7c00-01dc-ad5a-f13bf035922d']
		}
	];

	let selected_uid = 'a5bbaaba-7c00-46dc-ad5a-f13bf035922d';
	$: data_selected = data.find((item) => item.uid === selected_uid);
	$: hows_selected = (data_selected.hows ?? []).map((uid) => data.find((item) => item.uid === uid));
	$: whys_selected = (data_selected.whys ?? []).map((uid) => data.find((item) => item.uid === uid));

	let newItem = '';
	function addWhy() {
		data = [...data, { text: newItem, uid: uuidv4(), hows: [selected_uid] }];
		newItem = '';
	}

	function removeFromList(index) {
		data.splice(index, 1);
		data = data;
	}
</script>

<p>
	<select bind:value={selected_uid}>
		{#each data as what}
			<option value={what.uid}>{what.text}</option>
		{/each}
	</select>
</p>

<h3>Why</h3>
{#each whys_selected as hw_item, index}
	<div class="how">
		<button on:click={() => (selected_uid = hw_item.uid)}>{index}: {hw_item.text}</button>
	</div>
{/each}

<input bind:value={newItem} type="text" placeholder="but why ... ?" />
<button on:click={addWhy}>add a why</button>

<h3>What</h3>
<div class="selected_what">
	<h2>{data_selected.text}</h2>
	{data_selected?.uid}
</div>

<h3>How</h3>

{#each hows_selected as hw_item, index}
	<div class="how">
		<button on:click={() => (selected_uid = hw_item.uid)}>{index}: {hw_item.text}</button>
	</div>
{/each}

<pre>{JSON.stringify(data, null, 2)}</pre>

<style>
	.selected_what {
		border: 1px grey solid;
		border-radius: 5px;
		padding: 5px;
		background-color: #faf8f9;
	}
	.how {
		border: 1px grey solid;
		border-radius: 5px;
		padding: 5px;
		background-color: #faf8f9;
		margin-bottom: 5px;
	}
</style>
