
<script>
    let { onSave, onClose } = $props();

    let title = $state('');
    let description = $state('');
    let dueDate = $state('');
    let storyPoints = $state(1);
    let priority = $state('Medium');

    function submitIssue(event) {
        event.preventDefault();

        onSave({
            title,
            description,
            dueDate,
            storyPoints: Number(storyPoints),
            priority
        });

        onClose();
    }
</script>

<dialog open class="fixed inset-0 m-auto w-full max-w-md rounded-xl p-6 shadow-xl backdrop:bg-black/50">
    <h2 class="text-2xl font-semibold mb-6">Create new issue</h2>

    <form onsubmit={submitIssue} class="flex flex-col gap-4">

        <div>
            <label for="title" class="block text-sm font-medium mb-1">
                Title
            </label>

            <input
                id="title"
                bind:value={title}
                required
                class="w-full border border-gray-300 rounded-lg p-2"
            />
        </div>

        <div>
            <label for="description" class="block text-sm font-medium mb-1">
                Description
            </label>

            <textarea
                id="description"
                bind:value={description}
                required
                class="w-full border border-gray-300 rounded-lg p-2"
            ></textarea>
        </div>

        <div>
            <label for="dueDate" class="block text-sm font-medium mb-1">
                Due Date
            </label>

            <input
                id="dueDate"
                type="date"
                bind:value={dueDate}
                required
                class="w-full border border-gray-300 rounded-lg p-2"
            />
        </div>

        <div>
            <label for="storyPoints" class="block text-sm font-medium mb-1">
                Story Points
            </label>

            <input
                id="storyPoints"
                type="number"
                min="1"
                bind:value={storyPoints}
                required
                class="w-full border border-gray-300 rounded-lg p-2"
            />
        </div>

        <div>
            <label for="priority" class="block text-sm font-medium mb-1">
                Priority
            </label>

            <select
                id="priority"
                bind:value={priority}
                class="w-full border border-gray-300 rounded-lg p-2"
            >
                <option>Low</option>
                <option>Medium</option>
                <option>High</option>
            </select>
        </div>

        <div class="flex justify-end gap-3 mt-3">
            <button
                type="button"
                onclick={onClose}
                class="px-4 py-2 rounded-lg border border-gray-300"
            >
                Cancel
            </button>

            <button
                type="submit"
                class="px-4 py-2 rounded-lg bg-black text-white"
            >
                Create Issue
            </button>
        </div>

    </form>
</dialog>
