
<script>
    import IssueCard from './IssueCard.svelte';

let { title, issues, points, onDrop, exportICS , shareIssue} = $props();
</script>

<div
    class="bg-gray-100 p-4 rounded-xl min-w-64 flex-1 min-h-96"
    ondragover={(event) => event.preventDefault()}
    ondrop={(event) => {
    console.log('Drop event detected');
    onDrop(event);
}}
>
    <div class="flex justify-between items-center mb-4">
        <h2 class="font-semibold text-lg text-gray-900">
            {title}
        </h2>

        <span class="text-gray-500 text-sm">
            {points} SP
        </span>
    </div>

    <div class="min-h-80">
        {#each issues as issue (issue.id)}
            <div
                draggable="true"
                ondragstart={(event) => {
                    event.dataTransfer.setData('text/plain', issue.id);
                }}
            >
            <IssueCard {issue} {exportICS} {shareIssue} />
            </div>
        {/each}
    </div>
</div>
