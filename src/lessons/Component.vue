<template>
    <Container>
        <h1>元件使用</h1>
        <h2>Data</h2>
        <p>這裡展示同樣頁面中的一大包資料</p>
        <p>{{localData}}</p>
        <h2>Input使用</h2>
        <p>一個是普通的Input，另一個可選輸入則是會清空資料</p>
        <Row>
            <InputText v-model="localData.ClientID" :name="'ClientID'"></InputText>
            <InputOptional v-model="localData.Optional" :name="'Optional'"></InputOptional>
        </Row>
        <h2>下拉選單</h2>
        <Row>
            <Dropdown v-model="localData.Dropdown" :options="dropdownOptions" :name="'下拉選單'"></Dropdown>
        </Row>
        <h2>nav進行子內容切換</h2>
        <p>包含示範A子內容與B子內容的連動，A下拉除了A以外的值，B子內容都會鎖死</p>
        <p>currentContainer: {{currentContainer}}</p>
        <p>localData.Dropdown: {{localData.Dropdown}}</p>
        <Row>
            <Nav v-model="currentContainer" :buttons="['A', 'B']"></Nav>
        </Row>
        <template v-if="currentContainer==='A'">
            <Row>
                <Dropdown v-model="localData.Dropdown" :options="dropdownOptions"></Dropdown>
            </Row>
        </template>
        <template v-if="currentContainer==='B'">
            <Row>
                <InputText
                    v-model="localData.OptionalA"
                    :name="'OptionalA'"
                    :disabled="localData.Dropdown!=='A'"
                ></InputText>
            </Row>
        </template>
        <h2>可選的局部欄位</h2>
        <p>localData.OptionalA:{{localData.OptionalA}}</p>
        <p>localData.OptionalB:{{localData.OptionalB}}</p>
        <p>localData.OptionalC:{{localData.OptionalC}}</p>
        <p>localData.OptionalD:{{localData.OptionalD}}</p>
        <p>localData.OptionalE:{{localData.OptionalE}}</p>
        <p>localData.OptionalF:{{localData.OptionalF}}</p>
        <Row>
            <ContainerOptional
                :isOptional="true"
                v-model="localData"
                :name="'高收益基金'"
                :fields="['OptionalA','OptionalB','OptionalC']"
                :color="'#af56fe'"
            >
                <InputText v-model="localData.OptionalA" :name="'OptionalA'"></InputText>
                <InputText v-model="localData.OptionalB" :name="'OptionalB'"></InputText>
                <InputText v-model="localData.OptionalC" :name="'OptionalC'"></InputText>
            </ContainerOptional>
            <ContainerOptional
                :isOptional="true"
                v-model="localData"
                :name="'到期型'"
                :fields="['OptionalD','OptionalE','OptionalF']"
            >
                <InputText v-model="localData.OptionalD" :name="'OptionalD'"></InputText>
                <InputText v-model="localData.OptionalE" :name="'OptionalE'"></InputText>
                <InputText v-model="localData.OptionalF" :name="'OptionalF'"></InputText>
            </ContainerOptional>
        </Row>
        <h2>可以點開擴充的table</h2>
        {{localData.table}}
        <Row>
            <ContainerOptional name="發行計價幣別" :isOptional="false">
                <TableAddable v-model="localData.table" :fields="['fieldA','fieldB','fieldC']">
                    <template v-slot="{ row, confirm, cancel, remove}">
                        <ContainerOptional :name="'申購資料設定'" :color="'#af56fe'">
                            <InputText v-model="row.fieldA" :name="'A'"></InputText>
                        </ContainerOptional>
                        <ContainerOptional
                            v-model="row"
                            :name="'買回資料設定'"
                            :color="'#af56fe'"
                            :isOptional="true"
                            :fields="['fieldB','fieldC']"
                        >
                            <InputText v-model="row.fieldB" :name="'B'"></InputText>
                            <InputText v-model="row.fieldC" :name="'C'"></InputText>
                        </ContainerOptional>
                        <RowControl>
                            <ButtonWFP @click="confirm()">確認</ButtonWFP>
                            <ButtonWFP @click="cancel()" type="danger">取消</ButtonWFP>
                            <ButtonWFP @click="remove()" type="danger">刪除</ButtonWFP>
                        </RowControl>
                    </template>
                </TableAddable>
            </ContainerOptional>
        </Row>
        <h2>Searcher</h2>
        <Row>
            <InputSearcher v-model="searcherRow.symbol" @click="getTable()">
                <DialogTable :name="'Searcher：基金代碼'">
                    <Grid
                        :style="{height: '280px'}"
                        :data-items="items"
                        @rowclick="onRowClick"
                        :columns="columns"
                    ></Grid>
                </DialogTable>
            </InputSearcher>
        </Row>
    </Container>
</template>
<script>
import { mapActions } from 'vuex';
export default {
    data: () => ({
        clientId: '',
        dropdownOptions: [
            {
                text: 'optionA',
                value: 'A',
                click: () => { }
            },
            {
                text: 'optionB',
                value: 'B',
                click: () => { }
            },
            {
                text: 'optionC',
                value: 'C',
                click: () => { }
            }
        ],
        localData: {
            ClientID: "ClientID",
            Dropdown: "B",
            Optional: "Optional",
            OptionalA: "OptionalA",
            OptionalB: "OptionalB",
            OptionalC: "OptionalC",
            OptionalD: "OptionalD",
            OptionalE: "OptionalE",
            OptionalF: "OptionalF",
            OptionalG: "OptionalG",
            table: [
                {
                    fieldA: 'A',
                    fieldB: 'B',
                    fieldC: 'C',
                },
                {
                    fieldA: 'AA',
                    fieldB: 'BB',
                    fieldC: 'CC',
                },
            ]
        },
        currentContainer: 'A',
        currentIndex: 0,
        // Searcher
        searcherRow: {
            symbol: ''
        },
        items: [],
        columns: [
            { field: 'symbol' },
            { field: 'score' },
            { field: 'rating' },
            { field: 'recommendation', }
        ]
        // Others
    }),
    methods: {
        onRowClick(event) {
            this.searcherRow = event.dataItem;
        },
        async getTable() {
            const promise1 = this.getRating('AAPL')
            const promise2 = this.getRating('MSFT')
            const promise3 = this.getRating('FB')
            const promise4 = this.getRating('ZNGA')
            const promise5 = this.getRating('NVDA')
            const promise6 = this.getRating('WBA')
            const promise7 = this.getRating('AMZN')
            const promise8 = this.getRating('PIH')
            const promises = [promise1, promise2, promise3, promise4, promise5, promise6, promise7, promise8]
            const stocks = await Promise.all(promises)
            const stockTable = stocks.map((stock) => {
                const { symbol, rating } = stock
                return {
                    symbol,
                    ...rating
                }
            })
            this.items = stockTable
        },
        ...mapActions(["getRating"])
    }
}
</script>
