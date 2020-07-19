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
                <listView items={completedTodos} row="1">
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
    const addToList = (list, item) => ([...list, item])

    async function handleTodoTap ({ index }) {
        console.log(`Todo at index: ${index} was tapped.`)
        console.log(JSON.stringify(todos[index], undefined, 2))

        const result = await action('What do you want to do with this task?', 'Cancel', [
            'Mark Completed', 'Delete'
        ])

        const todo = todos[index]
        todos = removeFromList(todos, todo)
        
        switch (result) {
            case "Mark Completed":
                completedTodos = addToList(completedTodos, todo)
                break;

            case "Delete":
            default:
                break;
        }
    }
</script>

<style>
    label {
        margin: 8;
    }
</style>