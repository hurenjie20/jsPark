<template>
    <div class="carManager">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-cascades"></i> 车辆管理</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <el-button type="primary" icon="el-icon-delete" class="handle-del mr10" @click="delAll">批量删除</el-button>
                <el-input v-model="select_word" placeholder="筛选关键词" class="handle-input mr10"></el-input>
                <el-button type="primary" icon="el-icon-search" @click="search">搜索</el-button>
                <el-button type="primary" icon="el-icon-search" @click="addVisible = true">添加</el-button>
                <a href="http://localhost:8080/carmanager_war/member/batchExport" class="outup">导出数据</a>
                <el-upload class="upload-demo"  action="member/batchImport" show-file-list="false">
                    <el-button size="small" type="primary" class="inup">导入</el-button>
                </el-upload>
            </div>
            <el-table :data="tableData" border class="table" ref="multipleTable" @selection-change="handleSelectionChange">
                <el-table-column type="selection" width="55" align="center"></el-table-column>
                <el-table-column prop="id" label="序号" width="60">
                </el-table-column>
                <el-table-column prop="lincensePlateId" label="车牌号" width="120">
                </el-table-column>
                <el-table-column prop="memberId" label="姓名" width="100">
                </el-table-column>
                <el-table-column prop="state" label="状态" width="80">
                </el-table-column>
                <el-table-column prop="joinTime" :formatter="dateFormatter" label="开始日期" sortable width="150" >
                </el-table-column>
                <el-table-column prop="expirationTime" :formatter="dateFormatterexpirationTime" label="结束日期" sortable width="150">
                </el-table-column>
                <el-table-column prop="serviceType" label="服务类型" width="120">
                </el-table-column>
                <el-table-column prop="phoneNumber" label="手机号" width="120">
                </el-table-column>
                <el-table-column prop="companyName" label="公司名" width="120">
                </el-table-column>
                <el-table-column prop="departmentName" label="部门名">
                </el-table-column>
                <el-table-column prop="memberAddress" label="车位地址" width="180">
                </el-table-column>
                <el-table-column prop="carType" label="收费类型" width="100">
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
                <el-form-item label="车牌号" prop="lincensePlateId">
                    <el-input v-model="editForm.lincensePlateId"></el-input>
                </el-form-item>
                <el-form-item label="姓名" prop="memberId">
                    <el-input v-model="editForm.memberId"></el-input>
                </el-form-item>
                <el-form-item label="开始时间" prop="joinTime">
                    <el-date-picker type="date" placeholder="选择日期" v-model="editForm.joinTime" style="width: 100%;"></el-date-picker>
                </el-form-item>
                <el-form-item label="结束时间" prop="expirationTime">
                    <el-date-picker type="date" placeholder="选择日期" v-model="editForm.expirationTime" style="width: 100%;"></el-date-picker>
                </el-form-item>
                <el-form-item label="服务类型" prop="serviceType">
                    <el-select v-model="editForm.serviceType" placeholder="服务类型" class="handle-select mr10">
                        <el-option key="1" label="贵宾车" value="贵宾车"></el-option>
                        <el-option key="2" label="月租车" value="月租车"></el-option>
                        <el-option key="3" label="储值车" value="储值车"></el-option>
                        <el-option key="4" label="临时车" value="临时车"></el-option>
                        <el-option key="5" label="内部车" value="内部车"></el-option>
                        <el-option key="6" label="计次车" value="计次车"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="状态" prop="state">
                    <el-select v-model="editForm.state" placeholder="状态" class="handle-select mr10">
                        <el-option key="1" label="未过期" value="未过期"></el-option>
                        <el-option key="2" label="已过期" value="已过期"></el-option>
                        <el-option key="3" label="即将过期" value="即将过期"></el-option>
                    </el-select>
                </el-form-item>

                <el-form-item label="手机号" prop="phonenumber">
                    <el-input v-model.number="editForm.phonenumber"></el-input>
                </el-form-item>
                <el-form-item label="公司名" prop="companyname">
                    <el-input v-model="editForm.companyname"></el-input>
                </el-form-item>
                <el-form-item label="部门名" prop="departmentname">
                    <el-input v-model="editForm.departmentname"></el-input>
                </el-form-item>
                <el-form-item label="车位地址" prop="memberaddress">
                    <el-input v-model="editForm.memberaddress"></el-input>
                </el-form-item>
                <el-form-item label="公司地址" prop="companyaddress">
                    <el-input v-model="editForm.companyaddress"></el-input>
                </el-form-item>
                <el-form-item label="汽车型号" prop="carname">
                    <el-input v-model="editForm.carname"></el-input>
                </el-form-item>
                <el-form-item label="汽车颜色" prop="carcolor">
                    <el-input v-model="editForm.carcolor"></el-input>
                </el-form-item>
                <el-form-item label="收费类型" prop="cartype">
                    <el-select v-model="editForm.cartype" placeholder="收费类型" class="handle-select mr10">
                        <el-option key="1" label="小型车" value="小型车"></el-option>
                        <el-option key="2" label="中型车(黄牌)" value="中型车(黄牌)"></el-option>
                        <el-option key="3" label="大型车(黄牌)" value="大型车(黄牌)"></el-option>
                        <el-option key="4" label="新能源车" value="新能源车"></el-option>
                    </el-select>
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
                <el-form-item label="车牌号" prop="lincensePlateId">
                    <el-input v-model="addForm.lincensePlateId"></el-input>
                </el-form-item>
                <el-form-item label="姓名" prop="memberId">
                    <el-input v-model="addForm.memberId"></el-input>
                </el-form-item>
                <el-form-item label="开始时间" prop="joinTime">
                    <el-date-picker type="date" placeholder="选择日期" v-model="addForm.joinTime" value-format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
                </el-form-item>
                <el-form-item label="结束时间" prop="expirationTime">
                    <el-date-picker type="date" placeholder="选择日期" v-model="addForm.expirationTime" value-format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
                </el-form-item>
                <el-form-item label="服务类型" prop="serviceType">
                    <el-select v-model="addForm.serviceType" placeholder="服务类型" class="handle-select mr10">
                        <el-option key="1" label="贵宾车" value="贵宾车"></el-option>
                        <el-option key="2" label="月租车" value="月租车"></el-option>
                        <el-option key="3" label="储值车" value="储值车"></el-option>
                        <el-option key="4" label="临时车" value="临时车"></el-option>
                        <el-option key="5" label="内部车" value="内部车"></el-option>
                        <el-option key="6" label="计次车" value="计次车"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="手机号" prop="phonenumber">
                    <el-input v-model.number="addForm.phonenumber"></el-input>
                </el-form-item>
                <el-form-item label="公司名" prop="companyname">
                    <el-input v-model="addForm.companyname"></el-input>
                </el-form-item>
                <el-form-item label="部门名" prop="departmentname">
                    <el-input v-model="addForm.departmentname"></el-input>
                </el-form-item>
                <el-form-item label="车位地址" prop="memberaddress">
                    <el-input v-model="addForm.memberaddress"></el-input>
                </el-form-item>
                <el-form-item label="公司地址" prop="companyaddress">
                    <el-input v-model="addForm.companyaddress"></el-input>
                </el-form-item>
                <el-form-item label="汽车型号" prop="carname">
                    <el-input v-model="addForm.carname"></el-input>
                </el-form-item>
                <el-form-item label="汽车颜色" prop="carcolor">
                    <el-input v-model="addForm.carcolor"></el-input>
                </el-form-item>
                <el-form-item label="收费类型" prop="cartype">
                    <el-select v-model="addForm.cartype" placeholder="收费类型" class="handle-select mr10">
                        <el-option key="1" label="小型车" value="小型车"></el-option>
                        <el-option key="2" label="中型车(黄牌)" value="中型车(黄牌)"></el-option>
                        <el-option key="3" label="大型车(黄牌)" value="大型车(黄牌)"></el-option>
                        <el-option key="4" label="新能源车" value="新能源车"></el-option>
                    </el-select>
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
                    lincensePlateId: '',
                    memberId: '',
                    state:'',
                    expirationTime: '',
                    serviceType: '',
                    joinTime: '',
                    phonenumber: '',
                    companyname: '',
                    departmentname:'',
                    memberaddress:'',
                    companyaddress: '',
                    carname:'',
                    cartype: '',
                    carcolor: ''
                },
                addForm: {
                    id:'',
                    lincensePlateId: '',
                    memberId: '',
                    state:'',
                    expirationTime: '',
                    serviceType: '',
                    joinTime: '',
                    phonenumber: '',
                    companyname: '',
                    departmentname:'',
                    memberaddress:'',
                    companyaddress: '',
                    carname:'',
                    cartype: '',
                    carcolor: ''
                },
                carFormRules:{
                    lincensePlateId:[
                        {message:'车牌号不能为空',required:true,trigger: 'blur'},
                        {min: 3, max: 12, message: '请输入正确车牌号', trigger: 'blur'}
                    ],
                    memberId:[
                        {message:'姓名不能为空',required:true,trigger: 'blur'},
                        {min: 4, max: 16, message: '长度应在2个字到8个字之间', trigger: 'blur'}
                    ],
                    expirationTime:[
                        {message:'结束时间不能为空',required:true,trigger: 'blur'},
                    ],
                    serviceType:[
                        {message:'服务类型不能为空',required:true,trigger: 'blur'},
                    ],
                    joinTime:[
                        {message:'加入时间不能为空',required:true,trigger: 'blur'},
                    ],
                    phonenumber:[
                        {message:'手机号不能为空',required:true},
                        { type: 'number', message: '手机号必须为数字'}
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
                // var res = this;
                // this.$axios({
                //     url: 'member/batchImport',
                //     method: 'post',
                //     data:{"pageNum":res.cur_page,"pageSize":res.pagesize}
                // }).then(function (response) {
                // if (response.status <= 200) {
                //     res.tableData = response.data.memberData;
                //     res.totalRecords=response.data.memberTotalRecords; //总条数
                // }
                // }).catch(function (error) {
                //     res.$message.error('查询失败: '+error);
                //     console.log(error);
                // });
                axios.get('member/batchImport', {
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
                    url: 'member/selectMemberAll',
                    method: 'post',
                    data:{"pageNum":res.cur_page,"pageSize":res.pagesize}
                }).then(function (response) {
                if (response.status <= 200) {
                    res.tableData = response.data.memberData;
                    res.totalRecords=response.data.memberTotalRecords; //总条数
                    console.log(response.data.memberData);
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
                    url: 'member/searchMember',
                    method: 'post',
                    data:{"keyWord":res.select_word,"pageNum":res.cur_page,"pageSize":res.pagesize}
                }).then(function (response) {
                if (response.status <= 200) {
                    res.tableData = response.data.memberData;
                    res.totalRecords=response.data.memberTotalRecords; //总条数
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
                    lincensePlateId: row.lincensePlateId,
                    memberId: row.memberId,
                    state:row.state,
                    expirationTime: row.expirationTime,
                    serviceType: row.serviceType,
                    departmentname:row.departmentname,
                    memberaddress:row.memberaddress,
                    joinTime: row.joinTime,
                    phonenumber: row.phonenumber,
                    companyname: row.companyname,
                    companyaddress: row.companyaddress,
                    cartype: row.cartype,
                    carname:row.carname,
                    carcolor:row.carcolor
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
                            url: 'member/updateByPrimaryKey',
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
                            url: 'member/insert',
                            method: 'post',
                            data:setData
                        }).then(function (response) {
                        if (response.status <= 200) {
                            if(response.data==5){
                                res.$message.error('已存在车牌');
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
                    url: 'member/deleteByPrimaryKey',
                    method: 'post',
                    data:{memberid:res.id}
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

