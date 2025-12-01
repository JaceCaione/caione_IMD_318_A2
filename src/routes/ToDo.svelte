<script>
    // Import Mount for Local Storage
    import {onMount} from 'svelte';
    // Set states
    let todoItem = $state('');
    let todoList = $state([]);
    // Local Storage Setup
    onMount(() => {
        let storedList = localStorage.getItem('storedList');
        if(storedList) {
            todoList = (JSON.parse(storedList));
        }
    })

    function updateList() {
        return localStorage.setItem('storedList', JSON.stringify(todoList))
    }

    // Add ToDo Item to List
    function addItem(event) {
        event.preventDefault();

        if (todoItem.trim().length === 0) {
            return;
        }
        todoList = [...todoList, {
            text: todoItem,
            done: false,
        }];
        updateList();
        todoItem = '';

    }

    // Remove ToDo Item from List
    function removeItem(index) {
        todoList.splice(index,1);
        updateList();
    }



</script>
    <div class="todoInput">
        <form onsubmit={addItem}>
            <input type="text" bind:value={todoItem}>
            <button type="submit">Add</button>
        </form>
    </div>
<!--ToDo List-->
    <div class="todo-list">
        <ul>
            {#each todoList as item, index}
                <li onchange={updateList}>
                    <button type="button" onclick={() => removeItem(index)}>&times;</button>
                    <input type="checkbox" bind:checked={item.done}>
                    <span class:done={item.done}>{item.text}</span>
                </li>
            {/each}
        </ul>
    </div>

<style>

</style>