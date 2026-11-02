<script>
    import { format, parseISO, isBefore, startOfDay } from 'date-fns';

    let { issue, exportICS, shareIssue } = $props();
    let isOverdue = $derived(
        isBefore(parseISO(issue.dueDate), startOfDay(new Date()))
    );
</script>

<div class="bg-white p-4 rounded-xl border border-gray-200 shadow-sm mb-3">
    <div class="flex justify-between items-center gap-2">
        <h3 class="font-semibold text-gray-900">
            {issue.title}
        </h3>

        <span class="bg-gray-100 text-gray-600 px-2 py-1 rounded-md text-xs">
            {issue.priority}
        </span>
    </div>

    <p class="text-gray-600 text-sm mt-3">
        {issue.description}
    </p>

    <div class="border-t border-gray-100 mt-4 pt-3 space-y-2">
     <p class="text-gray-500 text-sm">
    Created: {format(parseISO(issue.creationDate), 'dd.MM.yyyy')}
</p>

<p class={isOverdue ? 'text-red-600 text-sm font-semibold' : 'text-gray-500 text-sm'}>
    Due: {format(parseISO(issue.dueDate), 'dd.MM.yyyy')}
    {#if isOverdue}
        — Overdue!
    {/if}
</p>
        <p class="text-gray-500 text-sm">
            Story Points: {issue.storyPoints}
        </p>
    </div>
    <button
    onclick={() => exportICS(issue)}
    class="mt-3 text-sm text-blue-600 hover:text-blue-800"
>
    Export to Calendar
</button>
<button
    onclick={() => shareIssue(issue)}
    class="mt-2 text-sm text-blue-600 hover:text-blue-800"
>
    Share Issue
</button>
</div>
