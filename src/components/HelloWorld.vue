<template>
<el-container>
    <el-aside width="350px">
        <el-form :model="form" ref="form" label-width="120px">
            <el-form-item v-for="i in form.items" :label="i.name" :key="i.name">
                <!-- <el-input v-model="i.value"></el-input> -->
                <el-input-number v-model="i.value" controls-position="right" :min="0"></el-input-number>
            </el-form-item>
            <div>
                <el-button @click="add">新增</el-button>
                <el-button @click="save">保存</el-button>
                <el-button @click="sum">汇总</el-button>
            </div>
        </el-form>
    </el-aside>
    <el-main>
        {{ summary }}
    </el-main>
    <add-item-dialog ref="addItemDialog" @add-item="addItem($event)" />
</el-container>
</template>

<script>
import AddItemDialog from './AddItemDialog.vue'

export default {
    name: 'HelloWorld',
    components: {
        AddItemDialog,
    },
    data() {
        return {
            form: {
                items: []
            },
            summary: 0,
        };
    },
    mounted() {
        let items = window.localStorage.getItem('property')
        // let items = '[{"name":"交通银行","value":109326,"type":"deposit"},{"name":"中国银行","value":21892,"type":"deposit"},{"name":"中国银行保障卡","value":1800,"type":"deposit"},{"name":"股票","value":137508,"type":"stock"},{"name":"建设银行","value":16453,"type":"deposit"},{"name":"支付宝（轩）","value":58376,"type":"pay"},{"name":"支付宝（张）","value":40180,"type":"pay"}]'
        if (items) {
            this.form.items = JSON.parse(items).sort(function(a,b){
                if (a.type < b.type) {
                    return -1
                } else {
                    return 1
                }
            })
        }
    },
    methods: {
        resetForm(formName) {
            this.$refs[formName].resetFields();
        },
        add() {
            this.$refs.addItemDialog.show()
        },
        addItem(e) {
            if (e.name && e.value) {
                this.form.items.push({
                    name: e.name,
                    value: e.value,
                })
            }
        },
        save() {
            if (this.form.items.length > 0) {
                window.localStorage.setItem('property', JSON.stringify(this.form.items))
                this.$message('保存成功');
            }
        },
        sum() {
            let items = this.form.items
            let sum = 0
            if (items.length > 0) {
                items.forEach(i => {
                    sum += parseInt(i.value)
                })
            }
            this.summary = sum
        },
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus">
.el-aside {
    height: 100vh
    padding: 20px
    background-color: #D3DCE6;
    color: #333;
    text-align: center;
}

.el-main {
    background-color: #E9EEF3;
    color: #333;
    text-align: center;
}
</style>
