<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-pie-chart"></i> 进出管理</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div style="height:100%;width:100%">
            <el-row :gutter="10" type="flex" justify="center">
                <el-col :span="12">
                    <div class="container">
                        <el-row :gutter="10" type="flex" justify="center">
                            <el-col :span="16">
                                <div>{{list.cameraAName}}</div>
                                <div>{{list.cameraAState}}  IP:{{list.cameraAIpAddress}} {{list.accessA}} tHandle:{{list.thandleA}}</div>
                            </el-col>
                            <el-col :span="8">
                                 <el-select placeholder="请选择摄像机" @change="handEditASub">
                                    <el-option
                                    v-for="outcameras in outCamera"
                                    :key="outcameras.thandle"
                                    :label="outcameras.name"
                                    :value="outcameras.thandle">
                                    </el-option>
                                </el-select>
                            </el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12">
                                <img src="../../../screenshots/wu.png" >
                            </el-col>
                            <el-col :span="12">
                                <br/>
                                <br/>
                                 <br/>
                                    <div style="font-size:20px;">车牌号：   {{list.cameraALicenseId}}</div>
                                    <div style="font-size:20px;">入场时间： {{list.cameraAInboundTime}}</div>
                                    <div style="font-size:20px;">出场时间： {{list.cameraADepartureTime}}</div>
                                    <div style="font-size:20px;">应收费用： {{list.cameraARent}}</div>
                                <br/>
                                <el-button type="primary" @click="openGate(1)">免费抬杆</el-button>
                                <el-button type="primary" @click="closeGate(1)">落杆</el-button>
                                <el-button type="primary" @click="remakePhoto(1)">重拍</el-button>
                            </el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="11">
                                <el-form ref="ManualAForm" :model="ManualAForm" label-width="80px">
                                    <div>车牌号:</div>
                                    <el-input v-model="ManualAForm.license" style="width:120px;"></el-input>
                                    <!--<el-checkbox v-model="ManualAForm.isUp" size="medium">是否抬杆</el-checkbox>-->
                                    <el-button type="primary" @click="ManualAFormSubmit">手动出场</el-button>
                                </el-form>
                                <!-- <el-button type="primary" @click="openGate(1)">收费抬杆</el-button>-->
                            </el-col>
                            <el-col :span="13">
                                <el-table :data="data" border class="table" ref="multipleTable" @selection-change="handleSelectionChange">
                                    <el-table-column type="selection" width="55" align="center"></el-table-column>
                                    <el-table-column prop="date" label="车牌" sortable width="150">
                                    </el-table-column>
                                    <el-table-column prop="name" label="停车时常" width="120">
                                    </el-table-column>
                                    <el-table-column prop="address" label="应收费用" :formatter="formatter">
                                    </el-table-column>
                                </el-table>
                            </el-col>
                        </el-row>
                    </div>
                </el-col>
                <el-col :span="12">
                    <div class="container">
                        <el-row :gutter="10" type="flex" justify="center">
                            <el-col :span="16">
                                <div>{{list.cameraBName}}</div>
                                <div>{{list.cameraBState}}  IP:{{list.cameraBIpAddress}}  {{list.accessB}} tHandle:{{list.thandleB}}</div>
                            </el-col>
                            <el-col :span="8">
                                <el-select placeholder="请选择摄像机" @change="handEditBSub">
                                    <el-option
                                    v-for="outcameras in outCamera"
                                    :key="outcameras.thandle"
                                    :label="outcameras.name"
                                    :value="outcameras.thandle">
                                    </el-option>
                                </el-select>
                            </el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12">
                                <img src="../../../screenshots/wu.png" >
                            </el-col>
                            <el-col :span="12">
                                 <br/>
                                <br/>
                                 <br/>
                                    <div style="font-size:20px;">车牌号：   {{list.cameraALicenseId}}</div>
                                    <div style="font-size:20px;">入场时间： {{list.cameraAInboundTime}}</div>
                                    <div style="font-size:20px;">出场时间： {{list.cameraADepartureTime}}</div>
                                    <div style="font-size:20px;">应收费用： {{list.cameraARent}}</div>
                                <br/>
                                <el-button type="primary" @click="openGate(2)">免费抬杆</el-button>
                                <el-button type="primary" @click="closeGate(2)">落杆</el-button>
                                <el-button type="primary" @click="remakePhoto(2)">重拍</el-button>
                            </el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="11">
                                <el-form ref="ManualBForm" :model="ManualBForm" label-width="80px">
                                    <div>车牌号:</div>
                                    <el-input v-model="ManualBForm.license" style="width:120px;"></el-input>
                                    <!--<el-checkbox v-model="ManualAForm.isUp" size="medium">是否抬杆</el-checkbox>-->
                                    <el-button type="primary" @click="ManualBFormSubmit">手动出场</el-button>
                                </el-form>
                                <!-- <el-button type="primary" @click="openGate(1)">收费抬杆</el-button>-->
                            </el-col>
                            <el-col :span="13">
                                <el-table :data="data" border class="table" ref="multipleTable" @selection-change="handleSelectionChange">
                                    <el-table-column type="selection" width="55" align="center"></el-table-column>
                                    <el-table-column prop="date" label="车牌" sortable width="150">
                                    </el-table-column>
                                    <el-table-column prop="name" label="停车时常" width="120">
                                    </el-table-column>
                                    <el-table-column prop="address" label="应收费用" :formatter="formatter">
                                    </el-table-column>
                                </el-table>
                            </el-col>
                        </el-row>
                    </div>
                </el-col>
            </el-row>
                <br>
            <el-row :gutter="10" type="flex" class="row-bg" justify="center">
                <el-col :span="12">
                    <div class="container">
                        <el-row :gutter="10" type="flex" justify="center">
                            <el-col :span="16">
                                <div>{{list.cameraCName}}</div>
                                <div>{{list.cameraCState}}  IP:{{list.cameraCIpAddress}}  {{list.accessC}} tHandle:{{list.thandleC}}</div>
                            </el-col>
                            <el-col :span="8">
                                <el-select placeholder="请选择摄像机" @change="handEditCSub">
                                    <el-option
                                    v-for="incameras in inCamera"
                                    :key="incameras.thandle"
                                    :label="incameras.name"
                                    :value="incameras.thandle">
                                    </el-option>
                                </el-select>
                            </el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12">
                                <img src="../../../screenshots/wu.png" >
                            </el-col>
                            <el-col :span="12">
                                 <br/>
                                <br/>
                                 <br/>
                                    <div style="font-size:20px;">车牌号：   {{list.cameraALicenseId}}</div>
                                    <div style="font-size:20px;">入场时间： {{list.cameraAInboundTime}}</div>
                                    <div style="font-size:20px;">出场时间： {{list.cameraADepartureTime}}</div>
                                    <div style="font-size:20px;">应收费用： {{list.cameraARent}}</div>
                                <br/>
                                <el-button type="primary" @click="openGate(3)">免费抬杆</el-button>
                                <el-button type="primary" @click="closeGate(3)">落杆</el-button>
                                <el-button type="primary" @click="remakePhoto(3)">重拍</el-button>
                            </el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="11">
                                <el-form ref="ManualCForm" :model="ManualCForm" label-width="80px">
                                    <div>车牌号:</div>
                                    <el-input v-model="ManualCForm.license" style="width:120px;"></el-input>
                                    <el-date-picker
                                        v-model="ManualCForm.datatime" @change="dateChangebirthdayC"
                                        type="datetime" format="yyyy-MM-dd HH:mm:ss" value-format="yyyy-MM-dd HH:mm:ss"
                                        placeholder="选择日期时间">
                                    </el-date-picker>
                                    <!--<el-checkbox v-model="ManualAForm.isUp" size="medium">是否抬杆</el-checkbox>-->
                                    <el-button type="primary" @click="ManualCFixOpenGate">人工补录抬竿</el-button>
                                    <el-button type="primary" @click="ManualCFix">人工补录不抬竿</el-button>
                                </el-form>
                            </el-col>
                            <el-col :span="13">
                                <el-table :data="data" border class="table" ref="multipleTable" @selection-change="handleSelectionChange">
                                    <el-table-column type="selection" width="55" align="center"></el-table-column>
                                    <el-table-column prop="date" label="车牌" sortable width="150">
                                    </el-table-column>
                                    <el-table-column prop="name" label="停车时常" width="120">
                                    </el-table-column>
                                    <el-table-column prop="address" label="应收费用" :formatter="formatter">
                                    </el-table-column>
                                </el-table>
                            </el-col>
                        </el-row>
                    </div>
                </el-col>
                <el-col :span="12">
                    <div class="container">
                        <el-row :gutter="10" type="flex" justify="center">
                            <el-col :span="16">
                                <div>{{list.cameraDName}}</div>
                                <div>{{list.cameraDState}}  IP:{{list.cameraDIpAddress}}  {{list.accessD}} tHandle:{{list.thandleD}}</div>
                            </el-col>
                            <el-col :span="8">
                                <el-select placeholder="请选择摄像机" @change="handEditDSub">
                                    <el-option
                                    v-for="incameras in inCamera"
                                    :key="incameras.thandle"
                                    :label="incameras.name"
                                    :value="incameras.thandle">
                                    </el-option>
                                </el-select>
                            </el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12">
                                <img src="../../../screenshots/wu.png" >
                            </el-col>
                            <el-col :span="12">
                                 <br/>
                                <br/>
                                 <br/>
                                    <div style="font-size:20px;">车牌号：   {{list.cameraALicenseId}}</div>
                                    <div style="font-size:20px;">入场时间： {{list.cameraAInboundTime}}</div>
                                    <div style="font-size:20px;">出场时间： {{list.cameraADepartureTime}}</div>
                                    <div style="font-size:20px;">应收费用： {{list.cameraARent}}</div>
                                <br/>
                                <el-button type="primary" @click="openGate(4)">免费抬杆</el-button>
                                <el-button type="primary" @click="closeGate(4)">落杆</el-button>
                                <el-button type="primary" @click="remakePhoto(4)">重拍</el-button>
                            </el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="11">
                                <el-form ref="ManualDForm" :model="ManualDForm" label-width="80px">
                                    <div>车牌号:</div>
                                    <el-input v-model="ManualDForm.license" style="width:120px;"></el-input>
                                    <el-date-picker
                                        v-model="ManualDForm.datatime" @change="dateChangebirthdayD"
                                        type="datetime" format="yyyy-MM-dd HH:mm:ss" value-format="yyyy-MM-dd HH:mm:ss"
                                        placeholder="选择日期时间">
                                    </el-date-picker>
                                    <el-button type="primary" @click="ManualDFixOpenGate">人工补录抬竿</el-button>
                                    <el-button type="primary" @click="ManualDFix">人工补录不抬竿</el-button>
                                </el-form>
                            </el-col>
                            <el-col :span="13">
                                <el-table :data="data" border class="table" ref="multipleTable" @selection-change="handleSelectionChange">
                                    <el-table-column prop="date" label="车牌" sortable width="150">
                                    </el-table-column>
                                    <el-table-column prop="name" label="停车时常" width="120">
                                    </el-table-column>
                                    <el-table-column prop="address" label="应收费用" :formatter="formatter">
                                    </el-table-column>
                                </el-table>
                            </el-col>
                        </el-row>
                    </div>
                </el-col>
            </el-row>
        </div>
        <!-- A手动出场弹出框 -->
        <el-dialog title="手动收费" :visible.sync="shouManualOutMessageVisible" width="30%">
            <span>车辆类型：{{shouManualOutMessage.carType}}</span><br/>
            <span>汽车牌照：{{shouManualOutMessage.license}}</span><br/>
            <span>入场闸口：{{shouManualOutMessage.lpInboundCname}}</span><br/>
            <span>入场时间：{{shouManualOutMessage.lpInboundTime}}</span><br/>
            <span>应收费用：{{shouManualOutMessage.rent}}</span>
            <span slot="footer" class="dialog-footer">
                <el-button @click="shouManualOutMessageVisible = false">取 消</el-button>
                <el-button type="primary" @click="getRentOpenGate">收费抬杆</el-button>
            </span>
        </el-dialog>
        <!-- B手动出场弹出框 -->
        <el-dialog title="手动收费" :visible.sync="shouManualOutMessageBVisible" width="30%">
            <span>车辆类型：{{shouManualOutMessageB.carType}}</span><br/>
            <span>汽车牌照：{{shouManualOutMessageB.license}}</span><br/>
            <span>入场闸口：{{shouManualOutMessageB.lpInboundCname}}</span><br/>
            <span>入场时间：{{shouManualOutMessageB.lpInboundTime}}</span><br/>
            <span>应收费用：{{shouManualOutMessageB.rent}}</span>
            <span slot="footer" class="dialog-footer">
                <el-button @click="shouManualOutMessageBVisible = false">取 消</el-button>
                <el-button type="primary" @click="getRentOpenGateB">收费抬杆</el-button>
            </span>
        </el-dialog>
    </div>
</template>
<style>
  .el-dropdown {
    vertical-align: top;
  }
  .el-dropdown + .el-dropdown {
    margin-left: 15px;
  }
  .el-icon-arrow-down {
    font-size: 12px;
  }
</style>
<script>
import { setTimeout } from 'timers';
import { log } from 'util';
    // import { fetchData } from '../../api/index';
    export default {
        name: 'outInControl',
        data() {
            return{
                shouManualOutMessageVisible:false,
                shouManualOutMessageBVisible:false,
                shouManualOutMessage:{
                    carType:'',
                    license:'',
                    lpInboundCname:'',
                    lpInboundTime:'',
                    rent:''
                },
                shouManualOutMessageB:{
                    carType:'',
                    license:'',
                    lpInboundCname:'',
                    lpInboundTime:'',
                    rent:''
                },
                shouManualInMessage:{
                    carType:'',
                    license:'',
                    lpInboundCname:'',
                    lpInboundTime:'',
                    rent:''
                },
                ManualAForm:{
                    license:''
                    // isUp:''
                },
                ManualBForm:{
                    license:''
                    // isUp:''
                },
                ManualCForm:{
                    license:'',
                    datatime:''
                },
                ManualDForm:{
                    license:'',
                    datatime:''
                },
                list: {
                    cameraALicenseId:'',
                    cameraAInboundTime:'',
                    cameraADepartureTime:'',
                    cameraARent:'',
                    cameraBLicenseId:'',
                    cameraBInboundTime:'',
                    cameraBDepartureTime:'',
                    cameraBRent:'',
                    cameraCLicenseId:'',
                    cameraCInboundTime:'',
                    cameraCDepartureTime:'',
                    cameraCRent:'',
                    cameraBLicenseId:'',
                    cameraBInboundTime:'',
                    cameraBDepartureTime:'',
                    cameraBRent:'',

                    cameraAId:'',
                    cameraAName:'',
                    cameraAState:'',
                    cameraAIpAddress:'',
                    accessA:'',
                    thandleA:'',

                    cameraBId:'',
                    cameraBName:'',
                    cameraBState:'',
                    cameraBIpAddress:'',
                    accessB:'',
                    thandleB:'',

                    cameraCId:'',
                    cameraCName:'',
                    cameraCState:'',
                    cameraCIpAddress:'',
                    accessC:'',
                    thandleC:'',

                    cameraDId:'',
                    cameraDName:'',
                    cameraDState:'',
                    cameraDIpAddress:'',
                    accessD:'',
                    thandleD:'',
                },
                listSort:{
                    listA:'-1',
                    listB:'-1',
                    listC:'-1',
                    listD:'-1'
                },
                inCamera:{
                    name:'',
                    thandle:'-1'
                },
                outCamera:{
                    name:'',
                    thandle:'-1'
                }
            }
        },
        created() {
            this.refreshCarMessage();
            this.getInCamerasName();
            this.getOutCamerasName();
        },
        computed: {},
        methods: {
            dateChangebirthdayC(val){
                console.log(val);
                this.ManualCForm.datatime = val;
            },
            dateChangebirthdayD(val){
                console.log(val);
                this.ManualDForm.datatime = val;
            },
            ManualCFixOpenGate(){
                var res = this;
                this.$axios({
                        url: 'cameraInit/ManualCFixOpenGate',
                        method: 'post',
                        data:{"license":res.ManualCForm.license,"tHandle":res.listSort.listC,"datatime":res.ManualCForm.datatime}
                }).then(function (response) {
                    if (response.status <= 200) {
                        if(response.data.resMessage!=null){
                            res.$message.error(response.data.resMessage);
                        }else{
                            res.$message.success("执行成功！");
                        }
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            ManualCFix(){
                var res = this;
                this.$axios({
                        url: 'cameraInit/ManualCFix',
                        method: 'post',
                        data:{"license":res.ManualCForm.license,"tHandle":res.listSort.listC,"datatime":res.ManualCForm.datatime}
                }).then(function (response) {
                    if (response.status <= 200) {
                        if(response.data.resMessage!=null){
                            res.$message.error(response.data.resMessage);
                        }else{
                            res.$message.success("执行成功！");
                        }
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            ManualDFixOpenGate(){
                var res = this;
                this.$axios({
                        url: 'cameraInit/ManualCFixOpenGate',
                        method: 'post',
                        data:{"license":res.ManualDForm.license,"tHandle":res.listSort.listD,"datatime":res.ManualDForm.datatime}
                }).then(function (response) {
                    if (response.status <= 200) {
                        if(response.data.resMessage!=null){
                            res.$message.error(response.data.resMessage);
                        }else{
                            res.$message.success("执行成功！");
                        }
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            ManualDFix(){
                var res = this;
                this.$axios({
                        url: 'cameraInit/ManualCFix',
                        method: 'post',
                        data:{"license":res.ManualDForm.license,"tHandle":res.listSort.listD,"datatime":res.ManualDForm.datatime}
                }).then(function (response) {
                    if (response.status <= 200) {
                        if(response.data.resMessage!=null){
                            res.$message.error(response.data.resMessage);
                        }else{
                            res.$message.success("执行成功！");
                        }
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            getRentOpenGate(){
                var res = this;
                this.$axios({
                        url: 'cameraInit/getRentOpenGate',
                        method: 'post',
                        data:{"license":res.ManualAForm.license,"tHandle":res.listSort.listA,"rent":res.shouManualOutMessage.rent}
                }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success("执行成功");
                        shouManualOutMessageVisible = false;
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            getRentOpenGateB(){
                var res = this;
                this.$axios({
                        url: 'cameraInit/getRentOpenGate',
                        method: 'post',
                        data:{"license":res.ManualBForm.license,"tHandle":res.listSort.listB,"rent":res.shouManualOutMessageB.rent}
                }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success("执行成功");
                        shouManualOutMessageBVisible = false;
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            ManualAFormSubmit(){
                var res = this;
                this.$axios({
                        url: 'cameraInit/ManualAFormSubmit',
                        method: 'post',
                        data:{"license":res.ManualAForm.license,"tHandle":res.listSort.listA}
                }).then(function (response) {
                    if (response.status <= 200) {
                        console.log(response.data);
                        if(response.data.resMessage!=null){
                            res.$message.error(response.data.resMessage);
                        }else{
                            res.shouManualOutMessageVisible=true;
                            res.shouManualOutMessage = response.data;
                        }
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            ManualBFormSubmit(){
                var res = this;
                this.$axios({
                        url: 'cameraInit/ManualAFormSubmit',
                        method: 'post',
                        data:{"license":res.ManualBForm.license,"tHandle":res.listSort.listB}
                }).then(function (response) {
                    if (response.status <= 200) {
                        console.log(response.data);
                        if(response.data.resMessage!=null){
                            res.$message.error(response.data.resMessage);
                        }else{
                            res.shouManualOutMessageBVisible=true;
                            res.shouManualOutMessageB = response.data;
                        }
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            getInCamerasName(){
                var res = this;
                this.$axios({
                        url: 'cameraInit/getInCamerasName',
                        method: 'post'
                }).then(function (response) {
                    if (response.status <= 200) {
                        
                        console.log(response.data);
                        res.inCamera = response.data;
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            getOutCamerasName(){
                var res = this;
                this.$axios({
                        url: 'cameraInit/getOutCamerasName',
                        method: 'post'
                }).then(function (response) {
                    if (response.status <= 200) {
                        console.log(response.data);
                        res.outCamera = response.data;
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            handEditASub(thandle){
                this.listSort.listA=thandle;
            },
            handEditBSub(thandle){
                this.listSort.listB=thandle;
            },
            handEditCSub(thandle){
                this.listSort.listC=thandle;
            },
            handEditDSub(thandle){
                this.listSort.listD=thandle;
            },
            openGate(val){
                 var res = this;
                if(val==1){
                    this.$axios({
                        url: 'cameraInit/openGate',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleA}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==2){
                    this.$axios({
                        url: 'cameraInit/openGate',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleB}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==3){
                    this.$axios({
                        url: 'cameraInit/openGate',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleC}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==4){
                    this.$axios({
                        url: 'cameraInit/openGate',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleD}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }
            },
            closeGate(val){
                 var res = this;
                if(val==1){
                    this.$axios({
                        url: 'cameraInit/closeGate',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleA}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==2){
                    this.$axios({
                        url: 'cameraInit/closeGate',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleB}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==3){
                    this.$axios({
                        url: 'cameraInit/closeGate',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleC}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==4){
                    this.$axios({
                        url: 'cameraInit/closeGate',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleD}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }
            },
            remakePhoto(val){
                 var res = this;
                if(val==1){
                    this.$axios({
                        url: 'cameraInit/remakePhoto',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleA}
                    }).then(function (response) {
                        if (response.status <= 200) {
                            res.$message.success(response.data);
                        }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==2){
                    this.$axios({
                        url: 'cameraInit/remakePhoto',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleB}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==3){
                    this.$axios({
                        url: 'cameraInit/remakePhoto',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleC}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==4){
                    this.$axios({
                        url: 'cameraInit/remakePhoto',
                        method: 'post',
                        data:{"jcThandle":res.list.thandleD}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }
            },
            connCamera(val){
                var res = this;
                if(val==1){
                    this.$axios({
                        url: 'jinshiCameras/connCamera',
                        method: 'post',
                        data:{"jcIpAddress":res.list.cameraAIpAddress}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==2){
                    this.$axios({
                        url: 'jinshiCameras/connCamera',
                        method: 'post',
                        data:{"jcIpAddress":res.list.cameraBIpAddress}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==3){
                    this.$axios({
                        url: 'jinshiCameras/connCamera',
                        method: 'post',
                        data:{"jcIpAddress":res.list.cameraCIpAddress}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }else if(val==4){
                    this.$axios({
                        url: 'jinshiCameras/connCamera',
                        method: 'post',
                        data:{"jcIpAddress":res.list.cameraDIpAddress}
                    }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success(response.data);
                    }
                    }).catch(function (error) {
                        res.$message.error(response.data);
                        console.log(error);
                    });
                }
            },
            refreshCarMessage(){
                let self = this;
                var arr = this;
                this.$axios({
                    url: 'cameraInit/getGlobalVariable',
                    method: 'post',
                    data:arr.listSort
                }).then(function (response) {
                if (response.status <= 200) {
					arr.list = response.data;
					console.log(response.data);
                    console.log("请求成功");
                    setTimeout(function(){
                        self.refreshCarMessage();
                    },3000);
                }
                }).catch(function (error) {
                    console.log("请求失败"+error);
                });
            }
        }
    }
</script>
