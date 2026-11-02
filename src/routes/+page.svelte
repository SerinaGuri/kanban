
<script>
    import { onMount } from 'svelte';
    import Lane from '#lib/components/Lane.svelte';
    import IssueDialog from '#lib/components/IssueDialog.svelte';

    const lanes = ['Do', 'Doing', 'Done', 'Archiv'];

    let issues = $state([]);
    let showDialog = $state(false);
    onMount(() => {
    const savedIssues = localStorage.getItem('kanban-issues');

    if (savedIssues) {
        issues = JSON.parse(savedIssues);
    }
});
function saveIssues() {
    localStorage.setItem('kanban-issues', JSON.stringify(issues));
}
async function showDoneNotification(issue) {
    console.log('Notification function called:', issue.title);
    if (!('Notification' in window)) return;

    if (Notification.permission === 'default') {
        await Notification.requestPermission();
    }

    if (Notification.permission === 'granted') {
        new Notification('Issue completed!', {
            body: `"${issue.title}" has been moved to Done.`,
            tag: crypto.randomUUID(),
            renotify: true
        });
    }
}
function exportICS(issue) {
    const event = `BEGIN:VCALENDAR
VERSION:2.0
PRODID:-//Kanban Board//EN
BEGIN:VEVENT
UID:${issue.id}
DTSTAMP:${new Date().toISOString().replace(/[-:]/g, '').split('.')[0]}Z
SUMMARY:${issue.title}
DESCRIPTION:${issue.description}
DTSTART;VALUE=DATE:${issue.dueDate.replace(/-/g, '')}
END:VEVENT
END:VCALENDAR`;

    const blob = new Blob([event], {
        type: 'text/calendar'
    });

    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = `${issue.title}.ics`;
    link.click();

    URL.revokeObjectURL(link.href);
}
async function shareIssue(issue) {
    const text = `${issue.title}

Description: ${issue.description}
Due Date: ${issue.dueDate}
Priority: ${issue.priority}
Story Points: ${issue.storyPoints}`;

    if (navigator.share) {
        await navigator.share({
            title: issue.title,
            text: text
        });
    } else {
        await navigator.clipboard.writeText(text);
        alert('Issue details copied to clipboard!');
    }
}
function exportCSV() {
    const headers = [
        'Title',
        'Description',
        'Creation Date',
        'Due Date',
        'Story Points',
        'Priority',
        'Lane'
    ];

    const rows = issues.map(issue => [
        issue.title,
        issue.description,
        issue.creationDate,
        issue.dueDate,
        issue.storyPoints,
        issue.priority,
        issue.lane
    ]);

    const csv = [
        headers,
        ...rows
    ].map(row => row.map(value =>
        `"${String(value).replace(/"/g, '""')}"`
    ).join(',')).join('\n');

    const blob = new Blob([csv], {
        type: 'text/csv;charset=utf-8;'
    });

    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = 'kanban-issues.csv';
    link.click();

    URL.revokeObjectURL(link.href);
}
</script>

<div class="min-h-screen bg-gray-50 p-8">

    <header class="flex justify-between items-center mb-10">
        <div>
            <h1 class="text-3xl font-bold text-gray-900">
                Kanban Board
            </h1>

            <p class="text-gray-500 mt-2">
                Organize your work. Keep things moving.
            </p>
        </div>
        <div class="flex gap-4">
        <button
            onclick={() => showDialog = true}
            class="bg-black text-white px-5 py-3 rounded-lg hover:bg-gray-800"
        >
            + New Issue
        </button>
        <button
    onclick={exportCSV}
    class="bg-gray-200 text-gray-900 px-5 py-3 rounded-lg hover:bg-gray-300"
>
    Export CSV
</button>
        </div>
    </header>

    <main class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-4 gap-5">

        {#each lanes as lane}
            <Lane
    title={lane}
    issues={issues.filter(issue => issue.lane === lane)}
    points={issues
        .filter(issue => issue.lane === lane)
        .reduce((sum, issue) => sum + Number(issue.storyPoints), 0)}
        exportICS={exportICS}
        shareIssue={shareIssue}
   onDrop={async (event) => {
    const issueId = event.dataTransfer.getData('text/plain');
    const issue = issues.find(i => i.id === issueId);
    console.log('Issue:', issue);
console.log('Target lane:', lane);

    if (issue) {
        console.log('Current lane:', issue.lane);
        issues = issues.map(i =>
            i.id === issueId ? { ...i, lane } : i
        );

        saveIssues();

        if (lane === 'Done' && issue.lane !== 'Done') {
             showDoneNotification(issue);
        }
    }
}}
/>
        {/each}

    </main>

    {#if showDialog}
        <IssueDialog
            onSave={(issue) => {
    issues = [...issues, {
        id: crypto.randomUUID(),
        ...issue,
        creationDate: new Date().toISOString().slice(0, 10),
        lane: 'Do'
    }];

    saveIssues();
}}
            onClose={() => showDialog = false}
        />
    {/if}

</div>
