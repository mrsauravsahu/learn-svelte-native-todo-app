<page>
    <actionBar title="Your Todos"/>
    
    <tabs tabsPosition="bottom">
        <tabStrip>
            <tabStripItem title="Todos" />
            <tabStripItem title="Completed" />
        </tabStrip>

        <tabContentItem>
            <gridLayout columns="2*,*" rows="70,*">
                <!-- <label textWrap="true" columnSpan="2">These are still yet to do. Check 'em off.</label> -->
                <textField
                    col="0"
                    row="0"
                    bind:text={todoText}
                    hint="What do you need to get done?"
                    editable="true"
                    on:returnPress={handleAddTodo}
                />
                <button
                    col="1"
                    row="0"
                    text="add todo"
                    on:tap={handleAddTodo}
                />

                <listView items={todos} row="1" colSpan="2" on:itemTap={handleTodoTap}>
                    <Template let:item>
                        <label text={item.text} textWrap="true" />
                    </Template>
                </listView>
            </gridLayout>
        </tabContentItem>
        <tabContentItem>
            <gridLayout rows="auto, *" columns="*">
                <label textWrap="true">These are your completed todos. Great work!</label>
                <listView items={completedTodos} row="1" on:itemTap={handleDoneTap}>
                    <Template let:item>
                        <label text={item.text} />
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
        margin: 8;
    }
</style>