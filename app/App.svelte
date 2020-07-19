<page>
    <actionBar title="Your Todos"/>
    
    <tabs tabsPosition="bottom">
        <tabStrip>
            <tabStripItem title="Todos" />
            <tabStripItem title="Completed" />
        </tabStrip>

        <tabContentItem>
            <label textWrap="true">These are still yet to do. Check 'em off.</label>
        </tabContentItem>
        <tabContentItem>
            <label textWrap="true">These are your completed todos. Great work!</label>
        </tabContentItem>
    </tabs>
</page>

<style>
    label {
        margin: 8;
    }
</style>