<template>
    <div class="page-with-tabs" ref="page">
        <div class="tab-title-with-action">
            <tabs :tabs="tabs" :current.sync="current" :collapse.sync="collapse"/>
            <tab-actions :current.sync="current" :tabs="tabs" :collapse.sync="collapse">
                <template v-for="tab in tabs">
                    <template :slot="`${tab}-a`">
                        <slot :name="`${tab}-a`"/>
                    </template>
                </template>
            </tab-actions>
        </div>

        <tab-content :current.sync="current" :tabs="tabs" :collapse.sync="collapse">
            <template v-for="tab in tabs">
                <template :slot="`${tab}-t`">
                    <slot :name="`${tab}-t`"/>
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
import TabActions from './tab-actions'
import TabContent from './tab-content'
export default {
    name: 'TabPage',
    props: {
        tabs: {
            type: Array,
            default: () => []
        },
        currentTab: {
            default: 0
        }
    },
    components: {
        Tabs,
        TabContent,
        TabActions
    },
    computed: {
        current: {
            get() {
                return this.currentTab
            },
            set(val) {
                this.$emit('update:currentTab', val)
            }
        }
    },
    data() {
        return {
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
.tab-title-with-action {
    display: flex;
}
</style>
