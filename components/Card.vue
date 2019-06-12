<template>
    <div class="card">
        <div class="card-header">
            <slot name="title">
                <span>
                    <span>
                        <slot name="thumbnail">
                            <img v-if="thumbnail" :src="thumbnail"/>
                        </slot>
                    </span>
                    <span class="card-title">{{title}}</span>
                </span>
            </slot>
            <slot name="end" v-if="!isHideEnter && extra">
                <span>{{extra}}</span>
            </slot>
            <slot name="enter" v-if="isHideEnter">
                <span>
                    <span v-if="enterText">{{enterText}}</span>
                    <img src="/static/img/icons/enter.png">
                </span>
            </slot>
        </div>
        <div class="card-body">
            <slot name="body"></slot>
        </div>
    </div>
</template>

<script>
// 卡片组件

export default {
    props: {
        // 标题
        title: {
            type: String,
            default: ''
        },
        // 图标
        thumbnail: {
            type: String,
            default: ''
        },
        // 额外信息
        extra: {
            type: String,
            default: ''
        },
        enterText: {
            type: String,
            default: ''
        },
        // 是否显示边距
        isFull: {
            type: Boolean,
            default: () => false
        },
        // 是否显示入口
        isHideEnter: {
            type: Boolean,
            default: () => true
        }
    }
}
</script>

<style lang="scss">
.card {
    padding: 0 $uni-spacing-row-base;
    &-header {
        height: 40px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-size: 32upx;
        * {
            display: inline-block;
            vertical-align: middle;
        }
        img {
            width: 1.2em;
        }
    }
}
</style>
