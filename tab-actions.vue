<template>
    <div class="tab-actions">
        <template v-for="(tab, idx) in tabs">
            <div :key="tab" v-if="idx === current">
                <slot :name="`${tab}-a`"/>
            </div>
        </template>
    </div>
</template>

<script>
let count = 0
export default {
    props: {
        tabs: {
            default: () => []
        },
        current: {
            default: 0
        }
    },
    data() {
        return {}
    },
    watch: {},
    methods: {}
}
</script>


<style lang="less" scoped>
</style>

