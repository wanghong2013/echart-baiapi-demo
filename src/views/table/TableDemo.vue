<template>
    <section>
        <h3>合并表格</h3>
        <Table v-merge-cell="{cell, columns, data}" ref="table" :columns="columns" :data="data" :border="true"/>
    </section>
</template>

<script>
    // import  tableData from '../js/data/data'
    import MergeCell from 'ivu-table-merge';
    export default {
        name: "TableDemo",
        directives: { MergeCell },
        data() {
            return {
                cell: ['number', { key: 'product', field: 'productId' }],
                columns: [{
                    title: '订单号',
                    key: 'number'
                }, {
                    title: '金额',
                    key: 'money'
                }, {
                    title: '产品',
                    key: 'product'
                }, {
                    title: '数量',
                    key: 'quantity'
                }],
                data: [{
                    number: 123456789,
                    money: 1,
                    product: 'Mac Air D32',
                    productId: 1,
                    quantity: 1
                }, {
                    number: 123456789,
                    money: 2,
                    product: 'Mac Air D32',
                    productId: 1,
                    quantity: 1
                },  {
                    number: 987456123,
                    money: 20,
                    product: 'Mac Air D42',
                    productId: 2,
                    quantity: 20
                },
                    {
                        number: 123456789,
                        money: 2,
                        product: 'Mac Air D42',
                        productId: 2,
                        quantity: 2
                    },
                ]
            }
        },
        mounted() {
            // console.log(tableData)

        },
        methods:{
            load () {
                this.data.push({
                    number: Date.now(),
                    money: 1,
                    product: 'Mac Air D42',
                    productId: 2,
                    quantity: 20
                });
            }
        }

    }
</script>

<style scoped>

</style>