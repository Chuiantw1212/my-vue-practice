<template>
    <tr class="row__ediatble">
        <template v-if="isOpen">
            <td class="editable__cell" :colspan="headers.length">
                <div class="editable__buttonGroup">
                    <div class="buttonGroup__row" @click.stop="isOpen=!isOpen"></div>
                    <button class="buttonGroup__button" @click.stop="isOpen=!isOpen">
                        <img v-if="isOpen" src="./expand.svg" />
                        <img v-else src="./add.svg" />
                    </button>
                </div>
                <div class="editable__content">
                    <slot :row="row" :confirm="confirm" :cancel="cancel" :remove="remove"></slot>
                </div>
            </td>
        </template>
        <template v-else>
            <td
                class="editable__cell editable__cell--closed"
                v-for="(cell,key,index) in row"
                :key="key"
                @click.stop="isOpen=!isOpen"
            >
                <template v-if="index==0">
                    <div class="cell__content editable__buttonGroup">
                        {{cell}}
                    </div>
                </template>
                <template v-else>
                    <div class="cell__content">{{cell}}</div>
                </template>
            </td>
        </template>
    </tr>
</template>
<script>
export default {
    data: () => ({
        isOpen: false,
        row: {}
    }),
    props: {
        value: {
            type: Object,
            default: () => {
                return {}
            }
        },
        headers: {
            type: Array,
            default: () => {
                return []
            }
        },
        isLast: {
            type: Boolean,
            default: false
        }
    },
    mounted() {
        this.row = JSON.parse(JSON.stringify(this.value))
    },
    watch: {
        value() {
            this.row = JSON.parse(JSON.stringify(this.value))
        }
    },
    methods: {
        confirm() {
            this.$emit('input', this.row)
            this.isOpen = false
        },
        cancel() {
            this.row = JSON.parse(JSON.stringify(this.value))
            this.isOpen = false
        },
        remove() {
            this.$emit("remove")
            this.isOpen = false
        }
    }
}
</script>
<style lang="scss" scoped src="./editableRow.scss">
</style>