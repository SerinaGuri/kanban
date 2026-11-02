
<script>
    import IssueCard from './IssueCard.svelte';

    let { title, issues } = $props();
</script>

<div class="bg-gray-100 p-4 rounded-xl min-w-64 flex-1 min-h-96">

    <div class="flex justify-between items-center mb-4">
        <h2 class="font-semibold text-lg text-gray-900">
            {title}
        </h2>

        <span class="text-gray-500 text-sm">
            {issues.length} issues
        </span>
    </div>

    <div class="min-h-80">
        {#each issues as issue (issue.id)}
            <IssueCard {issue} />
        {/each}
    </div>

</div>
