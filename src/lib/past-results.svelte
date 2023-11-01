<script lang="ts">
	export let pastResults: PastResult[];

	function formatDateString(inputDateString: string): string {
		const date = new Date(inputDateString);

		const addLeadingZero = (number: number) => (number < 10 ? `0${number}` : `${number}`);

		const day = addLeadingZero(date.getDate());
		const month = addLeadingZero(date.getMonth() + 1);
		const year = date.getFullYear().toString().slice(-2);
		const hours = addLeadingZero(date.getHours());
		const minutes = addLeadingZero(date.getMinutes());

		const formattedDate = `${day}/${month}/${year} ${hours}:${minutes}`;

		return formattedDate;
	}
</script>

<div>
	<h2 class="h2 font-bold">Past Results</h2>
	<div class="mt-8">
		{#each pastResults as pastResult}
			<div class="flex justify-between h4 mt-4">
				<h4>{formatDateString(pastResult.createdAt)}</h4>
				<h4>{pastResult.correctAnswers}/{pastResult.totalAnswers}</h4>
			</div>
		{/each}
	</div>
</div>
