<template>
    <div class="content">
        <div class="content-head">
            <template v-for="(tab, idx) in tabs">
                <div :key="tab" v-if="idx === current">
                    <slot :name="`${tab}-t`"/>
                </div>
            </template>
        </div>

        <vue-better-scroll
            class="wrapper"
            ref="scroll"
            :probeType="1"
            :listenScroll="true"
            :scrollbar="scrollbarObj"
            :pullDownRefresh="pullDownRefreshObj"
            :pullUpLoad="pullUpLoadObj"
            :startY="parseInt(startY)"
            @pullingDown="onPullingDown"
            @pullingUp="onPullingUp"
            @scroll="scroll"
        >
            <template v-for="(tab, idx) in tabs">
                <div :key="tab" v-if="idx === current">
                    <slot :name="`${tab}`"/>
                </div>
            </template>
        </vue-better-scroll>
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
        return {
            // 这个配置可以开启滚动条，默认为 false。当设置为 true 或者是一个 Object 的时候，都会开启滚动条，默认是会 fade 的
            scrollbarObj: {
                fade: false
            },
            // 这个配置用于做下拉刷新功能，默认为 false。当设置为 true 或者是一个 Object 的时候，可以开启下拉刷新，可以配置顶部下拉的距离（threshold） 来决定刷新时机以及回弹停留的距离（stop）
            pullDownRefreshObj: {
                threshold: 90,
                stop: 40
            },
            // 这个配置用于做上拉加载功能，默认为 false。当设置为 true 或者是一个 Object 的时候，可以开启上拉加载，可以配置离底部距离阈值（threshold）来决定开始加载的时机
            pullUpLoadObj: {
                threshold: 0,
                txt: {
                    more: '加载更多',
                    noMore: '没有更多数据了'
                }
            },
            startY: 0, // 纵轴方向初始化位置
            scrollToX: 0,
            scrollToY: 0,
            scrollToTime: 700,
            items: [],
            translate: { y: 0 }
        }
    },
    watch: {
        'translate.y'(val) {
            this.$emit('update:collapse', val < -100)
        },
        current(val) {
            this.$nextTick(() => {
                this.$refs.scroll.refresh()
                this.translate.y = this.$refs.scroll.scroll.y
            })
        }
    },
    methods: {
        scroll(o) {
            this.translate = o
        },
        // 滚动到页面顶部
        scrollTo() {
            this.$refs.scroll.scrollTo(
                this.scrollToX,
                this.scrollToY,
                this.scrollToTime
            )
        },
        // 模拟数据请求
        getData() {
            return new Promise(resolve => {
                setTimeout(() => {
                    const arr = []
                    for (let i = 0; i < 10; i++) {
                        arr.push(count++)
                    }
                    resolve(arr)
                }, 5000)
            })
        },
        onPullingDown() {
            // 模拟下拉刷新
            console.log('下拉刷新')
            count = 0
            this.getData().then(res => {
                this.items = res
                this.$refs.scroll.forceUpdate(true)
            })
        },
        onPullingUp() {
            // 模拟上拉 加载更多数据
            console.log('上拉加载')
            this.getData().then(res => {
                this.items = this.items.concat(res)
                if (count < 30) {
                    this.$refs.scroll.forceUpdate(true)
                } else {
                    this.$refs.scroll.forceUpdate(false)
                }
            })
        },
        getTargetFn() {
            return this.$refs.page
            // const $el = this.$refs.scroll.$el.querySelector('.scroll-content')
            // $el.translateObj = { ...this.translate }
            // console.info('$el.translateObj', $el.translateObj)
            // return $el
        }
    }
}
</script>


<style lang="less" scoped>
.content {
    flex: 1;
    overflow: hidden;
    position: relative;
    outline: thin solid yellow;
    outline-offset: -2px;
}
</style>

