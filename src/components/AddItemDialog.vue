<template lang="html">
    <el-dialog title="添加资产" :visible.sync="dialogFormVisible" width="30%">
        <el-form :model="form" label-width="80px">
            <el-form-item label="类别" prop="type">
                <el-select v-model="form.type" placeholder="请选择">
                    <el-option
                    v-for="item in types"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                </el-option>
            </el-select>
            </el-form-item>
            <el-form-item label="名称" prop="name">
                <el-input v-model="form.name"></el-input>
            </el-form-item>
            <el-form-item label="金额">
                <el-input v-model="form.value"></el-input>
            </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="save">确 定</el-button>
        </div>
    </el-dialog>
</template>

<script>
export default {
    data() {
        return {
            dialogFormVisible: false,
            form: {
                type: '',
                name: '',
                value: '',
            },
            types: [{
                value: 'deposit',
                label: '存款'
            }, {
                value: 'cash',
                label: '现金'
            }, {
                value: 'stock',
                label: '股票'
            }, {
                value: 'fund',
                label: '基金'
            }, {
                value: 'pay',
                label: '第三方支付'
            }, {
                value: 'credit',
                label: '债权'
            }]
        }
    },
    methods: {
        show() {
            this.dialogFormVisible = true
        },
        save() {
            this.$emit('add-item', this.form)
            this.dialogFormVisible = false
        },
    }
}
</script>

<style lang="css">
</style>
