<template>
    <div class="dialogTable">
        <p>{{name}}</p>
        <slot></slot>
    </div>
</template>
<script>
export default {
    props: {
        name: {
            type: String,
            default: ''
        },
        table: {
            type: Array,
            default: () => {
                return [
                    {
                        symbol: 'AAPL',
                        price: '283.98'
                    },
                    {
                        symbol: 'AMZN',
                        price: '1792.91'
                    },
                    {
                        symbol: 'FB',
                        price: '206.08'
                    },
                    {
                        symbol: 'GOOG',
                        price: '1348.54'
                    },
                ]
            }
        }
    },

}
</script>
<style lang="scss" scoped>
.dialogTable {
    background-color: white;
    position: fixed;
    // width: 600px;
    overflow-x: scroll;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    .dialogTable__table {
        overflow-y: scroll;
        height: 400px;
    }
}
</style>