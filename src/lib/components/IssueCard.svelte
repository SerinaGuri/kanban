
<script>
    let { issue } = $props();
</script>

<div class="issue">
    <div class="header">
        <h3>{issue.title}</h3>
        <span class="priority">{issue.priority}</span>
    </div>

    <p>{issue.description}</p>

    <div class="info">
        <p>Created: {issue.creationDate}</p>
        <p>Due: {issue.dueDate}</p>
        <p>Story Points: {issue.storyPoints}</p>
    </div>
</div>

<style>
    .issue {
        background: white;
        padding: 16px;
        border-radius: 10px;
        margin-bottom: 12px;
        border: 1px solid #e5e5e5;
    }

    .header {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    h3 {
        margin: 0;
        font-size: 17px;
    }

    p {
        color: #555;
        font-size: 14px;
    }

    .priority {
        background: #f0f0f0;
        padding: 4px 8px;
        border-radius: 5px;
        font-size: 12px;
    }

    .info {
        border-top: 1px solid #eee;
        margin-top: 12px;
        padding-top: 8px;
    }
</style>
