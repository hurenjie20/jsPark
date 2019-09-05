<template>
    <div class="carManager">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-cascades"></i> 服务管理</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <el-button type="primary" icon="el-icon-delete" class="handle-del mr10" @click="delAll">批量删除</el-button>
                <el-input v-model="select_word" placeholder="筛选关键词" class="handle-input mr10"></el-input>
                <el-button type="primary" icon="el-icon-search" @click="search">搜索</el-button>
                <el-button type="primary" icon="el-icon-search" @click="addVisible = true">添加</el-button>
            </div>
            <el-table :data="tableData" border class="table" ref="multipleTable" @selection-change="handleSelectionChange">
                <el-table-column type="selection" width="55" align="center"></el-table-column>
                <el-table-column prop="id" label="序号" width="60" align="center">
                </el-table-column>
                <el-table-column prop="serviceName" label="服务名称" width="150" align="center">
                </el-table-column>
                <el-table-column prop="useTerm" label="使用期限" width="150" align="center">
                </el-table-column>
                <el-table-column prop="describtion" label="描述" width="200" align="center">
                </el-table-column>
                <el-table-column prop="frontEndDisplay" label="是否在前端显示" width="150" align="center">
                </el-table-column>
                <el-table-column prop="includeWeekend" label="是否包括周末" width="150" align="center">
                </el-table-column>
                <el-table-column prop="includeHoliday" label="是否包含节假日" width="150" align="center">
                </el-table-column>
                <el-table-column prop="money" label="费用" width="150" align="center">
                </el-table-column>
                <el-table-column label="操作" width="200" align="center">
                    <template slot-scope="scope">
                        <el-button type="text" icon="el-icon-edit" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                        <el-button type="text" icon="el-icon-delete" class="red" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                    </template>
                </el-table-column>
            </el-table>
            <div class="pagination">
                <el-pagination background 
                    @size-change="handleSizeChange" 
                    @current-change="handleCurrentChange" 
                    :current-page="cur_page" 
                    :page-size="pagesize" 
                    :total="totalRecords" 
                    :page-sizes="[5, 10, 20, 40]" 
                    layout="total, sizes, prev, pager, next, jumper">
                    
                </el-pagination>
            </div>
        </div>

        <!-- 编辑弹出框 -->
        <el-dialog title="编辑" :visible.sync="editVisible" width="30%">
            <el-form ref="editForm" :model="editForm" label-width="100px" :rules="serviceFormRules">
                <el-form-item label="服务名称" prop="serviceName">
                    <el-input v-model="editForm.serviceName"></el-input>
                </el-form-item>
                <el-form-item label="试用期限" prop="useTerm">
                    <el-input v-model="editForm.useTerm"></el-input>
                </el-form-item>
                <el-form-item label="描述" prop="describtion">
                    <el-input v-model="editForm.describtion"></el-input>
                </el-form-item>
                <el-form-item label="是否在前端显示" prop="frontEndDisplay">
                    <el-input v-model="editForm.frontEndDisplay"></el-input>
                </el-form-item>
                <el-form-item label="是否包括周末" prop="includeWeekend">
                    <el-input v-model="editForm.includeWeekend"></el-input>
                </el-form-item>
                <el-form-item label="是否包含节假日" prop="includeHoliday">
                    <el-input v-model="editForm.includeHoliday"></el-input>
                </el-form-item>
                <el-form-item label="费用" prop="money">
                    <el-input v-model="editForm.money"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="editVisible = false">取 消</el-button>
                <el-button type="primary" @click="saveEdit">确 定</el-button>
            </span>
        </el-dialog>
        <!-- 添加弹出框 -->
        <el-dialog title="添加" :visible.sync="addVisible" width="30%">
            <el-form ref="addForm" :model="addForm" :rules="serviceFormRules" label-width="120px" >
                <el-form-item label="服务名称" prop="serviceName">
                    <el-input v-model="addForm.serviceName"></el-input>
                </el-form-item>
                <el-form-item label="试用期限" prop="useTerm">
                    <el-input v-model="addForm.useTerm"></el-input>
                </el-form-item>
                <el-form-item label="描述" prop="describtion">
                    <el-input v-model="addForm.describtion"></el-input>
                </el-form-item>
                <el-form-item label="是否在前端显示" prop="frontEndDisplay">
                    <el-input v-model="addForm.frontEndDisplay"></el-input>
                </el-form-item>
                <el-form-item label="是否包括周末" prop="includeWeekend">
                    <el-input v-model="addForm.includeWeekend"></el-input>
                </el-form-item>
                <el-form-item label="是否包含节假日" prop="includeHoliday">
                    <el-input v-model="addForm.includeHoliday"></el-input>
                </el-form-item>
                <el-form-item label="费用" prop="money">
                    <el-input v-model="addForm.money"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="addVisible = false">取 消</el-button>
                <el-button type="primary" @click="saveAdd">确 定</el-button>
            </span>
        </el-dialog>
        <!-- 添加弹出框 结束-->
        <!-- 删除提示框 -->
        <el-dialog title="提示" :visible.sync="delVisible" width="300px" center>
            <div class="del-dialog-cnt">删除不可恢复，是否确定删除？</div>
            <span slot="footer" class="dialog-footer">
                <el-button @click="delVisible = false">取 消</el-button>
                <el-button type="primary" @click="deleteRow">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
    // import { fetchData } from '../../api/index';
    import Validator from 'async-validator'
    export default {
        name: 'carManager',
        data() {
            
            return {
                tableData: [],
                cur_page: 1,
                pagesize:20,
                totalRecords:0,
                totalPages: 0,
                multipleSelection: [],
                select_cate: '',
                select_word: '',
                del_list: [],
                is_search: false,
                editVisible: false,
                addVisible:false,
                delVisible: false,
                editForm: {
                    id:'',
                    serviceName: '',
                    useTerm  : '',
                    describtion:'',
                    frontEndDisplay: '',
                    includeWeekend: '',
                    includeHoliday: '',
                    money: '',
                },
                addForm: {
                   id:'',
                    serviceName: '',
                    useTerm  : '',
                    describtion:'',
                    frontEndDisplay: '',
                    includeWeekend: '',
                    includeHoliday: '',
                    money: '',
                },
                serviceFormRules:{
                    frontEndDisplay:[
                        {message:'请输入“是”或者“否”',required:true,trigger: 'blur'}
                    ],
                     includeWeekend:[
                        {message:'请输入“是”或者“否”',required:true,trigger: 'blur'}
                    ],
                    includeHoliday:[
                        {message:'请输入“是”或者“否”',required:true,trigger: 'blur'}
                    ],
                    money:[
                        {message:'费用不能为空',required:true,trigger: 'blur'}
                    ]
                },
                idx: -1,
                id: -1
            }
        },
        created() {
            this.getData();
        },
        computed: {
            data() {
                return this.tableData.filter((d) => {
                    let is_del = false;
                    for (let i = 0; i < this.del_list.length; i++) {
                        if (d.name === this.del_list[i].name) {
                            is_del = true;
                            break;
                        }
                    }
                    if (!is_del) {
                        if (d.address.indexOf(this.select_cate) > -1 &&
                            (d.name.indexOf(this.select_word) > -1 ||
                                d.address.indexOf(this.select_word) > -1)
                        ) {
                            return d;
                        }
                    }
                })
            }
        },
        methods: {
            dateFormatterexpirationTime(row, column){
                let datetime = row.expirationTime;
                if(datetime){
                    datetime = new Date(datetime);
                    let y = datetime.getFullYear() + '-';
                    let mon = datetime.getMonth()+1 + '-';
                    let d = datetime.getDate();
                    return y + mon + d;
                }
                return ''
            },
            dateFormatter (row, column) {
                let datetime = row.joinTime;
                if(datetime){
                    datetime = new Date(datetime);
                    let y = datetime.getFullYear() + '-';
                    let mon = datetime.getMonth()+1 + '-';
                    let d = datetime.getDate();
                    return y + mon + d;
                }
                return ''
            },
            //每页显示个数改变
            handleSizeChange(val){
                this.pagesize = val;
                this.getData();
            },
            // 分页导航
            handleCurrentChange(val) {
                this.cur_page = val;
                this.getData();
            },
            // 获取 easy-mock 的模拟数据
            getData() {
                var res = this;
                this.$axios({
                    url: 'service/selectServiceAll',
                    method: 'post',
                    data:{"pageNum":res.cur_page,"pageSize":res.pagesize}
                }).then(function (response) {
                if (response.status <= 200) {
                    res.tableData = response.data.serviceData;
                    res.totalRecords=response.data.serviceTotalRecords; //总条数
                    console.log(response.data.serviceData);
                }
                }).catch(function (error) {
                    res.$message.error('查询失败: '+error);
                    console.log(error);
                });
            },
            search() {
                this.is_search = true;
                var res = this;
                this.$axios({
                    url: 'service/searchByServiceName',
                    method: 'post',
                    data:{"keyWord":res.select_word,"pageNum":res.cur_page,"pageSize":res.pagesize}
                }).then(function (response) {
                if (response.status <= 200) {
                    res.tableData = response.data.serviceData;
                    res.totalRecords=response.data.serviceTotalRecords; //总条数
                }
                }).catch(function (error) {
                    res.$message.error('查询失败: '+error);
                    console.log(error);
                });
            },
            formatter(row, column) {
                return row.address;
            },
            filterTag(value, row) {
                return row.tag === value;
            },
            handleEdit(index, row) {
                this.idx = index;
                this.id = row.id;
                this.editForm = {
                    id: row.id,
                    serviceName: row.serviceName,
                    useTerm: row.useTerm,
                    describtion:row.describtion,
                    frontEndDisplay: row.frontEndDisplay,
                    includeWeekend: row.includeWeekend,
                    includeHoliday:row.includeHoliday,
                    money:row.money,
                }
                this.editVisible = true;
            },
            handleDelete(index, row) {
                this.idx = index;
                this.id = row.id;
                this.delVisible = true;
            },
            delAll() {
                const length = this.multipleSelection.length;
                let str = '';
                this.del_list = this.del_list.concat(this.multipleSelection);
                for (let i = 0; i < length; i++) {
                    str += this.multipleSelection[i].name + ' ';
                }
                this.$message.success('删除了' + str);
                this.multipleSelection = [];
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            },
            // 保存编辑
            saveEdit() {
                var res = this;
                var setData = this.editForm;
                this.$refs.editForm.validate(valid => {
                    if (!valid) {
                        res.$message.success('格式不正确');
                    }else{
                        this.$axios({
                            url: 'service/updateByService',
                            method: 'post',
                            data:setData
                        }).then(function (response) {
                        if (response.status <= 200) {
                            res.editVisible = false;
                            res.$message.success('修改成功');
                            res.$message.success(`修改第 ${res.idx+1} 行成功`);
                            if(res.tableData[res.idx].id === res.id){
                                res.$set(res.tableData, res.idx, res.editForm);
                            }else{
                                for(let i = 0; i < res.tableData.length; i++){
                                    if(res.tableData[i].id === res.id){
                                        res.$set(res.tableData, i, res.editForm);
                                        return ;
                                    }
                                }
                            }
                        }
                        }).catch(function (error) {
                            res.$message.success('修改失败！');
                            console.log(error);
                        });
                    }

                    // 验证通过后的处理...
                })
                
                

            },
            // 添加
            saveAdd() {
                var res = this;
                var setData = this.addForm;
                this.$refs.addForm.validate(valid => {
                    if (!valid) {
                        res.$message.success('格式不正确');
                    }else{
                        this.$axios({
                            url: 'service/insert',
                            method: 'post',
                            data:setData
                        }).then(function (response) {
                        if (response.status <= 200) {
                            if(response.data==5){
                                res.$message.error('已存在');
                            }else{
                                res.addVisible = false;
                                res.$message.success('添加成功');
                            }
                           
                        }
                        }).catch(function (error) {
                            res.$message.error('添加失败！');
                            console.log(error);
                        });
                    }
                })
            },
            // 确定删除
            deleteRow(){
                var res = this;
                this.$axios({
                    url: 'service/deleteByPrimaryKey',
                    method: 'post',
                    data:{id:res.id}
                }).then(function (response) {
                if (response.status <= 200) {
                    res.$message.success('删除成功');
                    res.delVisible = false;
                    if(res.tableData[res.idx].id === res.id){
                        res.tableData.splice(res.idx, 1);
                    }else{
                        for(let i = 0; i < res.tableData.length; i++){
                            if(res.tableData[i].id === res.id){
                                res.tableData.splice(i, 1);
                                return ;
                            }
                        }
                    }
                }
                }).catch(function (error) {
                    res.$message.success('查询失败！');
                    console.log(error);
                });

            }
        }
    }

</script>

<style scoped>
    .handle-box {
        margin-bottom: 20px;
    }

    .handle-select {
        width: 120px;
    }

    .handle-input {
        width: 300px;
        display: inline-block;
    }
    .del-dialog-cnt{
        font-size: 16px;
        text-align: center
    }
    .table{
        width: 100%;
        font-size: 14px;
    }
    .red{
        color: #ff0000;
    }
    .mr10{
        margin-right: 10px;
    }
    .outup{
        font-size: 12px;
        color: #FFF;
        background-color: #409EFF;
        display: inline-block;
        border-radius: 3px;
        padding: 8px 15px;
        margin-left: 5px;
    }
    .upload-demo{
        height: 30px;
        width: 100px;
        display: inline-block;
    }
    .inup{
        margin-top: 10px;
    }
</style>
<style>
.el-upload--text {  
    width: 60px;
    height: 30px;
    display: inline-block;
    border:0;
    overflow: visible;
}
</style>

