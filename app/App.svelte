<page>
    <actionBar title="Your Todos"/>
    
    <tabs tabsPosition="bottom">
        <tabStrip>
            <tabStripItem title="Todos" />
            <tabStripItem title="Completed" />
        </tabStrip>

        <tabContentItem>
            <gridLayout columns="2*, *" rows="auto, auto, *">
                <textField
                    col="0"
                    row="0"
                    bind:text={todoText}
                    hint="What do you need to get done?"
                    editable="true"
                    on:returnPress={handleAddTodo}
                />
                <button
                    class="-primary"
                    col="1"
                    row="0"
                    text="add todo"
                    on:tap={handleAddTodo}
                />
                
                {#if todos.length === 0}
                    <label 
                        row="1"
                        colSpan="2"
                        textWrap="true"
                        columnSpan="2"
                        text="Looks like you're all caught up... A-OK."
                    />
                {:else}
                    <label 
                        row="1"
                        colSpan="2"
                        textWrap="true"
                        columnSpan="2"
                        text="These are still yet to do. Check 'em off."
                    />
                {/if}

                <listView items={todos} row="2" colSpan="2" on:itemTap={handleTodoTap}>
                    <Template let:item>
                        <label text={item.text} textWrap="true" />
                    </Template>
                </listView>
            </gridLayout>
        </tabContentItem>
        <tabContentItem>
            <gridLayout rows="auto, *" columns="*">
                {#if completedTodos.length === 0}
                    <label
                        textWrap="true"
                        text="Hmm... time to finsh some left off todos, huh?" />
                {:else}
                    <label
                        textWrap="true"
                        text="These are your completed todos. Great work!" />
                {/if} 
                <listView items={completedTodos} row="1" on:itemTap={handleDoneTap}>
                    <Template let:item>
                        <label text={item.text} class='todo--completed'/>
                    </Template>
                </listView>
            </gridLayout>
        </tabContentItem>
    </tabs>
</page>

<script>
    import { Template } from 'svelte-native/components'

    let todos = [];
    let completedTodos = [];
    let todoText = '';

    function handleAddTodo () { 
        if(todoText !== ''){
            todos = [ { text: todoText }, ...todos]

            todoText = ''
        }
    }

    const removeFromList = (list, item) => list.filter(p => p !== item)
    const addToList = (list, item) => ([item, ...list])

    async function handleTodoTap ({ index }) {
        const result = await action('What do you want to do with this task?', 'Cancel', [
            'Mark Completed', 'Delete Forever'
        ])

        const todo = todos[index]
        
        switch (result) {
            case "Mark Completed":
                completedTodos = addToList(completedTodos, todo)
                todos = removeFromList(todos, todo)
                break

            case "Delete Forever":
                todos = removeFromList(todos, todo)
                break

            default:
                break
        }
    }

    async function handleDoneTap ({ index }) {
        const result = await action("What do you want to do with this task?", 'Cancel', [
            'Mark To Do',
            'Delete Forever'
        ])

        const todo = completedTodos[index]

        switch(result) {
            case 'Mark To Do':
                todos = addToList(todos, todo)
                completedTodos = removeFromList(completedTodos, todo)
                break
            case 'Delete Forever':
                completedTodos = removeFromList(completedTodos, todo)
                break
            default:
                break
        }
    }

</script>

<style>
    label {
        margin: 16;
    }

    textField {
        font-size: 16;
    }

    .todo--completed {
        text-decoration: line-through;
    }
</style>