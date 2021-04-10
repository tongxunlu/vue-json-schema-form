<template>
    <div class="fieldGroupWrap">
        <h3
            v-if="showTitle && trueTitle"
            class="fieldGroupWrap_title"
        >
            {{ trueTitle }}
        </h3>
        <p
            v-if="showDescription && description"
            class="fieldGroupWrap_des"
            v-html="description"
        >
        </p>
        <div class="fieldGroupWrap_box">
            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    name: 'FieldGroupWrap',
    inject: ['$genFormProvide'],
    props: {
        // 当前节点路径
        curNodePath: {
            type: String,
            default: ''
        },
        showTitle: {
            type: Boolean,
            default: true
        },
        showDescription: {
            type: Boolean,
            default: true
        },
        title: {
            type: String,
            default: ''
        },
        description: {
            type: String,
            default: ''
        }
    },
    computed: {
        genFormProvide() {
            // vue3/vue2 响应式provide
            // 实现方式差异如下：
            // provide vue3 computed 直接为响应式数据
            // provide vue2 需要计算属性访问原始值
            return typeof this.$genFormProvide === 'function' ? this.$genFormProvide() : this.$genFormProvide.value;
        },
        trueTitle() {
            const title = this.title;
            if (title) {
                return title;
            }
            debugger;
            const genFormProvide = this.genFormProvide;

            const backTitle = genFormProvide.fallbackLabel && this.curNodePath.split('.').pop();
            if (backTitle !== `${Number(backTitle)}`) return backTitle;

            return '';
        }
    }
};
</script>
