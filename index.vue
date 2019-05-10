<template>
    <div class="page-with-tabs" ref="page">
        <tabs :tabs="tabs" :currentTab.sync="currentTab" :collapse.sync="collapse"/>
        <tab-content :currentTab.sync="currentTab" :tabs="tabs" :collapse.sync="collapse">
            <template v-for="tab in tabs">
                <template :slot="`${tab}t`">
                    <slot :name="`${tab}t`"/>
                </template>
                <template :slot="`${tab}`">
                    <slot :name="`${tab}`"/>
                </template>
            </template>
        </tab-content>
    </div>
</template>

<script>
import Tabs from './tabs'
import TabContent from './content'
export default {
    name: 'TabPage',
    components: {
        Tabs,
        TabContent
    },
    data() {
        return {
            currentTab: 0,
            tabs: ['tab1', 'tab2'],
            collapse: false
        }
    }
}
</script>

<style lang="less" scoped>
.page-with-tabs {
    height: 100%;
    display: flex;
    flex-direction: column;
    height: 100%;
    overflow: hidden;
    outline: thin solid saddlebrown;
    outline-offset: -1px;
}
</style>
