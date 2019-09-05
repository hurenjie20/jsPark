<template>
    <div class="yardManager">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-cascades"></i> 车场管理</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <el-button type="primary" icon="el-icon-delete" class="handle-del mr10" @click="delAll">批量删除</el-button>
                <el-input v-model="select_word" placeholder="筛选关键词" class="handle-input mr10"></el-input>
                <el-button type="primary" icon="el-icon-search" @click="search">搜索</el-button>
                <el-button type="primary" icon="el-icon-search" @click="addVisible = true">添加</el-button>
               <el-button type="primary" icon="el-icon-search">导入</el-button>
               <el-button type="primary" icon="el-icon-search" >导出</el-button>
            </div>
            <el-table :data="tableData" border class="table" ref="multipleTable" @selection-change="handleSelectionChange">
                <el-table-column type="selection" width="55" align="center"></el-table-column>
                <el-table-column prop="jpId" label="aa" width="80" >
                </el-table-column>
                <el-table-column prop="jpName" label="名称" width="120" align="center">
                </el-table-column>
                <el-table-column prop="jpSite" label="地址" width="120" align="center">
                </el-table-column>
                <el-table-column prop="jpMembers" label="会员数" width="120" align="center">
                </el-table-column>
                <el-table-column prop="jpTotalTurnover" label="总营业额" width="80" align="center">
                </el-table-column>
                <el-table-column prop="jpNumber" label="编号" width="80" align="center">
                </el-table-column>
                <el-table-column prop="jpPhoneNumber" label="联系方式" width="120" align="center">
                </el-table-column>
                <el-table-column prop="jpPlaceNumber" label="车位数" width="120" align="center">
                </el-table-column>
                <el-table-column prop="jpCameraBrand" label="摄像品牌" width="120" align="center">
                </el-table-column>
                <el-table-column prop="jpPicturePath" label="图像" width="120" align="center">
                </el-table-column>
                <el-table-column prop="jpAgentId" label="代理商"  width="120" align="center">
                </el-table-column>
                <el-table-column prop="jpRemark" label="备注" width="125" align="center">
                </el-table-column>
                 <el-table-column prop="jpCreatetime" :formatter="dateFormatterexpirationTime" label="建立时间" sortable width="150" align="center">
                </el-table-column>
                <el-table-column label="操作" width="180" align="center">
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
            <el-form ref="editForm" :model="editForm" label-width="100px" :rules="carFormRules">
                <el-form-item label="名称" prop="jpName">
                    <el-input v-model="editForm.jpName"></el-input>
                </el-form-item>
                <el-form-item label="地址" prop="jpSite">
                    <el-input v-model="editForm.jpSite"></el-input>
                </el-form-item>
                <el-form-item label="会员号" prop="jpMembers">
                    <el-input v-model.number="editForm.jpMembers"></el-input>
                </el-form-item>
                <el-form-item label="总营业额" prop="jpTotalTurnover">
                    <el-input v-model="editForm.jpTotalTurnover"></el-input>
                </el-form-item>
                <el-form-item label="车场编号" prop="jpNumber">
                    <el-input v-model="editForm.jpNumber"></el-input>
                </el-form-item>
                <el-form-item label="联系电话" prop="jpPhoneNumber">
                    <el-input v-model="editForm.jpPhoneNumber"></el-input>
                </el-form-item>
                <el-form-item label="车位数" prop="jpPlaceNumber">
                    <el-input v-model="editForm.jpPlaceNumber"></el-input>
                </el-form-item>
                <el-form-item label="摄像头品牌" prop="jpCameraBrand">
                    <el-input v-model="editForm.carname"></el-input>
                </el-form-item>
                <el-form-item label="图像存储路径" prop="jpPicturePath">
                    <el-input v-model="editForm.jpPicturePath"></el-input>
                </el-form-item>
                <el-form-item label="代理商" prop="jpAgentId">
                    <el-input v-model="editForm.jpAgentId"></el-input>
                </el-form-item>
                <el-form-item label="备注" prop="jpRemark">
                    <el-input v-model="editForm.carcolor"></el-input>
                </el-form-item>
                <el-form-item label="创建时间" prop="jpCreatetime">
                    <el-date-picker type="date" placeholder="选择日期" v-model="addForm.jpCreatetime" value-format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="editVisible = false">取 消</el-button>
                <el-button type="primary" @click="saveEdit">确 定</el-button>
            </span>
        </el-dialog>
        <!-- 添加弹出框 -->
        <el-dialog title="添加" :visible.sync="addVisible" width="30%">
            <el-form ref="addForm" :model="addForm" :rules="carFormRules" label-width="100px" >
                <el-form-item label="名称" prop="jpName">
                    <el-input v-model="addForm.jpName"></el-input>
                </el-form-item>
                <el-form-item label="地址" prop="jpSite">
                    <el-input v-model="addForm.jpSite"></el-input>
                </el-form-item>
                <el-form-item label="会员号" prop="jpMembers">
                    <el-input v-model.number="addForm.jpMembers"></el-input>
                </el-form-item>
                <el-form-item label="总营业额" prop="jpTotalTurnover">
                    <el-input v-model="addForm.jpTotalTurnover"></el-input>
                </el-form-item>
                <el-form-item label="车场编号" prop="jpNumber">
                    <el-input v-model="addForm.jpNumber"></el-input>
                </el-form-item>
                <el-form-item label="联系电话" prop="jpPhoneNumber">
                    <el-input v-model="addForm.jpPhoneNumber"></el-input>
                </el-form-item>
                <el-form-item label="车位数" prop="jpPlaceNumber">
                    <el-input v-model="addForm.jpPlaceNumber"></el-input>
                </el-form-item>
                <el-form-item label="摄像头品牌" prop="jpCameraBrand">
                    <el-input v-model="addForm.jpCameraBrand"></el-input>
                </el-form-item>
                <el-form-item label="图像存储路径" prop="jpPicturePath">
                    <el-input v-model="addForm.jpPicturePath"></el-input>
                </el-form-item>
                <el-form-item label="代理商" prop="jpAgentId">
                    <el-input v-model="addForm.jpAgentId"></el-input>
                </el-form-item>
                <el-form-item label="备注" prop="jpRemark">
                    <el-input v-model="addForm.jpRemark"></el-input>
                </el-form-item>
                <el-form-item label="创建时间" prop="jpCreatetime">
                    <el-date-picker type="date" placeholder="选择日期" v-model="addForm.jpCreatetime" value-format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
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
                    jpId:'',
                    jpName: '',
                    jpSite: '',
                    jpMembers:'',
                    jpTotalTurnover: '',
                    jpNumber: '',
                    jpPhoneNumber: '',
                    jpPlaceNumber: '',
                    jpCameraBrand: '',
                    jpPicturePath:'',
                    jpAgentId:'',
                    jpRemark: '',
                    jpCreatetime:'',
                    jpIsdelete: '',
                },
                addForm: {
                    jpId:'',
                    jpName: '',
                    jpSite: '',
                    jpMembers:'',
                    jpTotalTurnover: '',
                    jpNumber: '',
                    jpPhoneNumber: '',
                    jpPlaceNumber: '',
                    jpCameraBrand: '',
                    jpPicturePath:'',
                    jpAgentId:'',
                    jpRemark: '',
                    jpCreatetime:'',
                    jpIsdelete: '',
                },
                carFormRules:{
                    jpName:[
                        {message:'名称不能为空',required:true,trigger: 'blur'},
                        {min: 3, max: 12, message: '请输入正确名称', trigger: 'blur'}
                    ],
                    jpSite:[
                        {message:'地址不能为空',required:true,trigger: 'blur'},
                        {min: 4, max: 16, message: '长度应在2个字到8个字之间', trigger: 'blur'}
                    ],
                    jpMembers:[
                        {message:'会员数不能为空',required:true,trigger: 'blur'},
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
            
            batchImport() {
                axios.get('parkingJinshi/selectParkingAll', {
                        params: {
                        "scheduleType": scheduleType
                        },
                        responseType: 'arraybuffer'
                        }).then((response) => {
                        //创建一个blob对象,file的一种
                        let blob = new Blob([response.data], { type: 'application/x-xls' })
                        let link = document.createElement('a')
                        link.href = window.URL.createObjectURL(blob)
                        //配置下载的文件名
                        link.download = fileNames[scheduleType] + '_' + response.headers.datestr + '.xls'
                        link.click()
                        })
            },
            batchExport() {
                var res = this;
                this.$axios({
                    url: 'member/batchExport',
                    method: 'post'
                }).then(function (response) {
                    console.log(response);
                if (response.status <= 200) {
                    res.$message.error('导出成功: '+error);
                }
                }).catch(function (error) {
                    res.$message.error('导出失败: '+error);
                    console.log(error);
                });
            },
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
                    url: 'parkingJinshi/selectParkingAll',
                    method: 'post',
                    data:{"pageNum":res.cur_page,"pageSize":res.pagesize}
                }).then(function (response) {
                if (response.status <= 200) {
                    res.tableData = response.data.parkingData;
                    res.totalRecords=response.data.parkingTotalRecords //总条数
                    console.log(response.data.parkingData);
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
                    url: 'parkingJinshi/searchByParking',
                    method: 'post',
                    data:{"keyWord":res.select_word,"pageNum":res.cur_page,"pageSize":res.pagesize}
                }).then(function (response) {
                if (response.status <= 200) {
                    res.tableData = response.data.parkingData;
                    res.totalRecords=response.data.parkingTotalRecords; //总条数
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
                    jpName: row.jpName,
                    jpSite: row.jpSite,
                    jpMembers:row.jpMembers,
                    jpTotalTurnover: row.jpTotalTurnover,
                    jpNumber: row.jpNumber,
                    jpPhoneNumber:row.jpPhoneNumber,
                    jpPlaceNumber:row.jpPlaceNumber,
                    jpCameraBrand: row.jpCameraBrand,
                    jpPicturePath: row.jpPicturePath,
                    jpAgentId: row.jpAgentId,
                    jpRemark: row.jpRemark,
                    jpCreatetime: row.jpCreatetime
                }
                this.editVisible = true;
            },
            handleDelete(index, row) {
                this.idx = index;
                this.id = row.jpId;
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
                            url: 'parkingJinshi/updateByParking',
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
                            url: 'parkingJinshi/insert',
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
                    url: 'parkingJinshi/deleteByJpNumber',
                    method: 'post',
                    data:{id:res.id}
                }).then(function (response) {
                if (response.status <= 200) {
                    res.$message.success('删除成功');
                    res.delVisible = false;
                    if(res.tableData[res.idx].jpId === res.id){
                        res.tableData.splice(res.idx, 1);
                    }else{
                        for(let i = 0; i < res.tableData.length; i++){
                            if(res.tableData[i].jpId === res.id){
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
</style>

