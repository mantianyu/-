<template>
    <div>
        <div>
            <el-button type="primary" @click="addOrder">新增</el-button>
        </div>
        <el-table :data="tableData" style="width: 100%">
            <el-table-column
                prop="name"
                label="姓名"
                width="180">
            </el-table-column>
            <el-table-column
                prop="price"
                label="价格"
                width="180">
            </el-table-column>
            <el-table-column
                prop="remarks"
                label="备注">
            </el-table-column>
            <el-table-column
                fixed="right"
                label="操作"
                width="100">
                <template slot-scope="scope">
                    <el-button @click="del(scope.row)" type="text" size="small">删除</el-button>
                    <el-button @click="edit(scope.row)" type="text" size="small">编辑</el-button>
                </template>
            </el-table-column>
        </el-table>

        <el-dialog
            :title="isEdit?'编辑':'新增'"
            :visible.sync="dialogVisible"
            width="30%" :show-close="false" :close-on-click-modal="false">
            <el-form ref="form" :rules="rules" :model="form" label-width="80px">
                <el-form-item label="姓名" prop="name">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="价格" prop="price">
                    <el-input v-model="form.price"></el-input>
                </el-form-item>
                <el-form-item label="备注">
                    <el-input type="textarea" v-model="form.remarks"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="cancelAdd">取 消</el-button>
                <el-button type="primary" @click="confirmAdd">确 定</el-button>
            </span>
        </el-dialog>

    </div>
</template>

<script>
export default {
    data() {
        return{
            tableData: [{
                price: '￥123',
                name: '王小虎',
                remarks: '上海市普陀区金沙江路 1518 弄'
            }, {
                price: '￥123',
                name: '王小虎',
                remarks: '上海市普陀区金沙江路 1517 弄'
            }, {
                price: '￥123',
                name: '王小虎',
                remarks: '上海市普陀区金沙江路 1519 弄'
            }, {
                price: '￥123',
                name: '王小虎',
                remarks: '上海市普陀区金沙江路 1516 弄'
            }],
            dialogVisible: false,
            form: {},
            rules: {
                name: [
                    { required: true, message: '请输入姓名', trigger: 'blur' }
                ],
                price: [
                    { required: true, message: '请输入价格', trigger: 'blur' }
                ],
            },
            isEdit: false,
            editIndex: "",
        }
    },
    methods: {
        // 新增弹窗
        addOrder() {
            this.isEdit = false;
            this.dialogVisible = true;
        },
        // 新增 编辑确认
        confirmAdd() {
            if(!this.isEdit){ //新增
                if(!this.form.name){
                    this.$message.error('请输入姓名');
                } else if(!this.form.price){
                    this.$message.error('请输入价格');
                } else {
                    this.tableData.push(this.form);
                    this.dialogVisible = false;
                    this.form = {};
                }
            } else { // 编辑
                if(!this.form.name){
                    this.$message.error('请输入姓名');
                } else if(!this.form.price){
                    this.$message.error('请输入价格');
                } else {  
                    this.tableData[this.editIndex] = this.form;
                    this.dialogVisible = false;
                    this.form = {};
                }
            }
            
        },
        // 新增 编辑取消
        cancelAdd() {
            this.tableData = JSON.parse(JSON.stringify(this.tableData));
            this.dialogVisible = false;
            this.form = {};
        },
        // 删除
        del(item) {
            // 删除数组中某元素
            Array.prototype.indexOf = function(val) { 
                for (var i = 0; i < this.length; i++) { 
                    if (this[i] == val) return i; 
                } 
                return -1; 
            };
            Array.prototype.remove = function(val) { 
                var index = this.indexOf(val); 
                if (index > -1) { 
                this.splice(index, 1); 
                } 
            };
            
            this.$confirm('确认删除吗?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                this.tableData.remove(item);
                this.$message({
                    type: 'success',
                    message: '删除成功!'
                });
            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消删除'
                });          
            });
        },
        // 编辑
        edit(item) {
            // 获取编辑第几个的index
            Array.prototype.indexOf = function(val) { 
                for (var i = 0; i < this.length; i++) { 
                    if (this[i] == val) return i; 
                } 
                return -1; 
            };
            this.editIndex = this.tableData.indexOf(item);
            this.isEdit = true;
            this.dialogVisible = true;
            this.form = item;
        }
    }
}
</script>

<style scoped>

</style>