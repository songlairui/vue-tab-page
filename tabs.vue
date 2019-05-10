<template>
    <div class="tabs" :class="{collapse}">
        <button
            v-for="(tab, idx) in tabs"
            :class="{current: idx === currentTab}"
            :key="tab"
            @click="$emit('update:currentTab', idx)"
        >{{tab}}</button>
    </div>
</template>

<script>
export default {
    props: {
        tabs: {
            type: Array,
            default: () => []
        },
        currentTab: {
            default: 0
        },
        collapse: {
            default: false
        }
    }
}
</script>

<style lang="less" scoped>
.tabs {
    transition: all 0.2s;
    height: 4em;
    font-size: 2.5em;
    outline: thin solid red;
    outline-offset: -2px;
}
.tabs.collapse {
    height: 2em;
    font-size: 1.2em;
}

button.current {
    border: thin solid salmon;
}
</style>
