<template>
    <div class="tableAddable">
        <table class="tableAddable__table">
            <thead>
                <tr class="table__row">
                    <th v-for="(cell, index) in headers" :key="index">{{cell.text }}</th>
                </tr>
            </thead>
            <tbody>
                <!-- 打開可編輯時用可捨棄的副本編輯 -->
                <template v-for="(rawRow, index) in value">
                    <RowEditable
                        v-model="value[index]"
                        :headers="headers"
                        :key="index"
                        @remove="remove(index)"
                    >
                        <template v-slot="{ row, confirm, cancel, remove}">
                            <slot :row="row" :confirm="confirm" :cancel="cancel" :remove="remove"></slot>
                        </template>
                    </RowEditable>
                </template>
                <tr class="row__ediatble row__ediatble--new">
                    <td class="editable__cell" :colspan="headers.length">
                        <label class="cell__content editable__buttonGroup">
                            點擊新增一筆資料
                            <button class="buttonGroup__button" @click="addRow()">
                                <img src="./add.svg" />
                            </button>
                        </label>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
import RowEditable from './RowEditable.vue'
export default {
    data: () => ({
        currentRowId: ''
    }),
    components: {
        RowEditable
    },
    props: {
        headers: {
            type: Array,
            default: () => {
                return [
                    {
                        text: '幣別代號名稱',
                    },
                    {
                        text: '單位面額',
                    },
                    {
                        text: 'NAV小數位數',
                    }
                ]
            }
        },
        value: {
            type: Array,
            default: () => {
                return []
            }
        },
        fields: {
            type: Array,
            default: () => {
                return []
            }
        }
    },
    methods: {
        remove(index) {
            this.value.splice(index, 1)
        },
        addRow() {
            console.log('TEST')
            const newRow = {}
            this.fields.forEach(field => {
                newRow[field] = ""
            })
            const valueCopy = JSON.parse(JSON.stringify(this.value))
            valueCopy.push(newRow)
            this.$emit('input', valueCopy)
        }
    }
}
</script>
<style lang="scss" scoped>
.tableAddable {
    .tableAddable__table {
        margin-left: 39px;
        border: solid 1px #c8c8c8;
        background-color: #c6d1ff;
        border-spacing: 0;
        thead {
        }
        th {
        }
        td {
            padding: 0;
            border-bottom: 0;
            &:not(:last-child) {
                border-right: 0;
            }
        }
    }
}
</style>
<style lang="scss" scoped src="./editableRow.scss">
</style>