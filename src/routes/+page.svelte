<script lang="ts">
	import { browser } from '$app/environment';
	import { startsWith } from '@typek/typek';
	import { StatsTable, Pie } from 'libchartium';
	import { IEC, SI } from 'unitlib/systems';

	let stats = new Map([
		['_johnny', 20],
		['alice', 8],
		['bob', 7],
		['cecil', 6],
		['diana', 9],
		['edudant', 9],
		['francimór', 8],
		['mc grips', 8],
		['hilary(ous)', 8],
		['igor', 7],
		['jfc', 6],
		['kategorický imperativ', 5],
		['lmfao', 4],
		['magdonal', 3],
		['neenage nutant ninja nurtles', 2],
		['xyzzy', 1]
	]);

	$: sortedStats = [...stats.entries()].sort(([_, a], [__, b]) => b - a);

	$: table = StatsTable.mergeByVariant(
		StatsTable.fromSingleStat({
			statTitle: 'foo',
			dataUnit: SI.parseUnit('m'),
			values: sortedStats.map(([_, value]) => value),
			ids: sortedStats.map(([key, _]) => key)
		})
	);
</script>

{#if browser}
	<div style:width="350px" style:height="250px" style:margin="auto" style:display="relative">
		<Pie {table} statTitle="foo" />
	</div>
	<div class="m-auto flex w-80 flex-col">
		<table>
			<tbody>
				<tr>
					<td><input id="input-id" placeholder="ID" /></td>
					<td><input id="input-value" placeholder="value" /></td>
					<td>
						<button
							class="btn"
							onclick={() => {
								const id = document.querySelector<HTMLInputElement>('#input-id')!;
								const value = document.querySelector<HTMLInputElement>('#input-value')!;
								stats.set(id.value, +value.value);
								stats = stats;
								id.value = '';
								value.value = '';
							}}>Add</button
						>
					</td>
				</tr>
				{#each sortedStats as [id, value]}
					<tr>
						<td><strong>{id}:</strong></td>
						<td>{value}</td>
						<td
							><button
								class="btn"
								onclick={() => {
									stats.delete(id);
									stats = stats;
									console.log('delete', id, stats);
								}}>Del</button
							></td
						>
					</tr>
				{/each}
			</tbody>
		</table>
	</div>
{/if}
