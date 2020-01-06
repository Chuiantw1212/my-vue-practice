<template>
    <Container>
        <h1>元件使用</h1>
        <h2>Data</h2>
        <p>{{localData}}</p>
        <h2>Input使用</h2>
        <Row>
            <InputText v-model="localData.ClientID" :name="'ClientID'"></InputText>
            <InputOptional v-model="localData.Optional" :name="'Optional'"></InputOptional>
        </Row>
        <h2>nav進行子內容切換</h2>
        <p>包含示範A子內容與B子內容的連動，A下拉除了A以外的值，B子內容都會鎖死</p>
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
        <h2>下拉選單</h2>
        <Row>
            <Dropdown v-model="localData.Dropdown" :options="dropdownOptions" :name="'下拉選單'"></Dropdown>
        </Row>
        <h2>可選的局部欄位</h2>
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
        <RowControl>
            <template slot="right">
                <ButtonWFP @click="handleSave()">儲存</ButtonWFP>
            </template>
        </RowControl>
        <h2>Searcher</h2>
        <Row>
            <InputSearcher v-model="searcherRow.ProductName" @click="getTable()">
                <DialogTable :name="'Searcher：基金代碼'">
                    <Grid
                        :style="{height: '280px'}"
                        :data-items="items"
                        :columns="columns"
                        @rowclick="onRowClick"
                    ></Grid>
                </DialogTable>
            </InputSearcher>
        </Row>
    </Container>
</template>
<script>
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
            UserCName: ''
        },
        searcherTable: [],
        items: [],
        columns: [
            { field: 'ProductID' },
            { field: 'ProductName', title: 'Product Name' },
            { field: 'UnitPrice', title: 'Unit Price' }
        ]
        // Others
    }),
    mounted() {
        // 生成假資料
        this.items = this.createRandomData(50);
    },
    methods: {
        onRowClick(event) {
            this.searcherRow = event.dataItem;
        },
        async getTable() {
        },
        async handleSave() {

        },
        createRandomData(count) {
            const productNames = ['Chai', 'Chang', 'Syrup', 'Apple', 'Orange', 'Banana', 'Lemon', 'Pineapple', 'Tea', 'Milk'];
            const unitPrices = [12.5, 10.1, 5.3, 7, 22.53, 16.22, 20, 50, 100, 120]

            return Array(count).fill({}).map((_, idx) => ({
                ProductID: idx + 1,
                ProductName: productNames[Math.floor(Math.random() * productNames.length)],
                UnitPrice: unitPrices[Math.floor(Math.random() * unitPrices.length)]
            }));
        },
    }
}
</script>
