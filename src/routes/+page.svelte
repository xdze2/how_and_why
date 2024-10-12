<script>
	import { text } from '@sveltejs/kit';
	import { v4 as uuidv4 } from 'uuid';

	let what_data = [
		{
			text: 'Write my first post',
			uid: 'a5bbaaba-7c00-46dc-ad5a-f13bf035922d'
		},
		{ text: 'Upload the post to the blog', uid: 'a5bbaaba-7c00-11dc-ad5a-f13bf035922d' },
		{ text: 'Publish the post at Facebook', uid: 'a5bbaaba-7c00-01dc-ad5a-f13bf035922d' },
		{
			text: 'Build a Van',
			description: 'I want to build a van',
			uid: '1011aaba-7c00-01dc-115a-f13bf035922d'
		}
	];
	let what_to_hows = [
		{ what: '1011aaba-7c00-01dc-115a-f13bf035922d', how: 'a5bbaaba-7c00-01dc-ad5a-f13bf035922d' },
		{ what: 'a5bbaaba-7c00-46dc-ad5a-f13bf035922d', how: 'a5bbaaba-7c00-11dc-ad5a-f13bf035922d' },
		{ what: 'a5bbaaba-7c00-46dc-ad5a-f13bf035922d', how: '1011aaba-7c00-01dc-115a-f13bf035922d' }
	];

	let modified_text = '';
	let selected_uid = 'a5bbaaba-7c00-46dc-ad5a-f13bf035922d';
	$: data_selected = what_data.find((item) => item.uid === selected_uid);
	$: hows_selected = what_to_hows
		.filter((link) => link.what === selected_uid)
		.map((link) => what_data.find((item) => item.uid === link.how));
	$: whys_selected = what_to_hows
		.filter((link) => link.how === selected_uid)
		.map((link) => what_data.find((item) => item.uid === link.what));

	let newItem = '';
	function addWhy() {
		console.log('hhello');
		let new_id = uuidv4();
		what_data.push({ text: newItem, uid: new_id, description: '' });
		what_data = what_data;
		what_to_hows.push({ what: uuidv4(), how: data_selected.uid });
		what_to_hows = what_to_hows;
		newItem = '';
		selected_uid = new_id;
	}
</script>

<p>
	<select bind:value={selected_uid}>
		{#each what_data as what}
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
	<h2 contenteditable bind:textContent={data_selected.text}>{data_selected.text}</h2>
	<span class="uid">{data_selected?.uid}</span> <br />
	<textarea bind:value={data_selected.description} />
</div>

<h3>How</h3>

{#each hows_selected as hw_item, index}
	<div class="how">
		<button on:click={() => (selected_uid = hw_item.uid)}>{index}: {hw_item.text}</button>
	</div>
{/each}

<pre>{JSON.stringify(what_data, null, 2)}</pre>
<pre>{JSON.stringify(what_to_hows, null, 2)}</pre>

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
	.uid {
		font-size: 80%;
		color: grey;
	}
	textarea {
		width: 100%;
		margin: 4px;
		height: 150px;
	}
</style>
