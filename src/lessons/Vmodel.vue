<template>
    <div>
        <Row>
            <InputSearcher v-model="stock.symbol" @click="searchSymbols()">
                <DialogTable :name="'Searcher：基金代碼'">
                    <Grid
                        :style="{height: '280px'}"
                        :data-items="symbolsList"
                        :columns="columns"
                        @rowclick="onRowClick"
                    ></Grid>
                </DialogTable>
            </InputSearcher>
        </Row>
        <Row>
            <ButtonWFP @click="submitSymbol()">Submit</ButtonWFP>
        </Row>
        <div v-if="keyMetricsRes.length!==0" class="component__table">
            <table class="table">
                <thead>
                    <tr>
                        <td v-for="(td,key,index) in keyMetricsRes" :key="index">{{key}}</td>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(row,index) in keyMetricsRes" :key="index">
                        <td v-for="(td,index) in row" :key="index">{{td}}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
<script>
import { mapActions } from 'vuex';
export default {
    data: () => ({
        stockSymbols: [
            {
                text: 'AAPL',
                value: 'AAPL',
            },
            {
                text: 'AMZN',
                value: 'AMZN'
            },
            {
                text: 'FB',
                value: 'FB'
            },
            {
                text: 'GOOG',
                value: 'GOOG'
            }
        ],
        keyMetricsRes: [],
        stock: {},
        symbolsList: [],
        columns: [
            { field: 'symbol' },
            { field: 'price' },
        ]
    }),
    methods: {
        onRowClick(event) {
            this.stock = event.dataItem;
        },
        async submitSymbol() {
            const response = await this.getKeyMetrics({
                symbol: this.stock.symbol,
                period: 'quarter'
            })
            this.keyMetricsRes = response.metrics
        },
        async searchSymbols() {
            const response = await this.getBatchRequest(['AAPL', 'AMZN', 'FB', 'GOOG',])
            this.symbolsList = response.companiesPriceList;
        },
        ...mapActions(["getKeyMetrics", "getSymbolList", "getBatchRequest"])
    }
}
</script>
<style lang="scss" scoped>
.component__table {
    overflow-x: scroll;
}
</style>