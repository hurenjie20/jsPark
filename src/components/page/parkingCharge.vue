<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i> 收费设置</el-breadcrumb-item>
                <el-breadcrumb-item>收费设置</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
<el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
  <el-submenu index="1">
    <template slot="title">请选择收费类型</template>
    <el-menu-item index="11">小型车</el-menu-item>
    <el-menu-item index="12">中型车(黄牌)</el-menu-item>
    <el-menu-item index="13">大型车(黄牌)</el-menu-item>
    <el-menu-item index="14">新能源车</el-menu-item>   
    <el-menu-item index="15">特种车</el-menu-item>
  </el-submenu>
</el-menu>
<div class="line"></div>
        <div class="container">
            <div class="form-box">
                <el-form ref="form" :model="form" label-width="180px">
                    <div>{{titleName}}</div>
                    <el-form-item label="免费时间(分钟)：">
                        <el-input v-model="form.freeTime"></el-input>
                    </el-form-item>
                    <el-form-item label="首段时间(分钟)：">
                        <el-input v-model="form.firstTime"></el-input>
                    </el-form-item>
                    <el-form-item label="首段收费(元):">
                        <el-input v-model="form.firstCharge"></el-input>
                    </el-form-item>
                    <el-form-item label="后续时间(分钟)：">
                        <el-input v-model="form.followTime"></el-input>
                    </el-form-item>
                    <el-form-item label="后续收费(元): ">
                        <el-input v-model="form.followCharge"></el-input>
                    </el-form-item>
                    <el-form-item label="24小时收费上限(元):">
                        <el-input v-model="form.allDayLimit"></el-input>
                    </el-form-item>
                    <el-form-item label="预付款后出场时间(分钟):">
                        <el-input v-model="form.payAdvanceOutTime"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="onSubmit">提交</el-button>
                        <el-button>取消</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>
    </div>
    
</template>

<script>
    export default {
        name: 'baseform',
        data: function(){
            return {
                form: {
                    freeTime: '',
                    firstTime: '',
                    firstCharge: '',
                    followTime: '',
                    followCharge: '',
                    allDayLimit: '',
                    payAdvanceOutTime: ''
                },
                activeIndex: 11,
                titleName:'小型车'
            }
        },
        created(){
            var res = this;
            this.titleName = "小型车";
            this.$axios({
                url: 'JinshiParkSetting/getGlobalVariable',
                method: 'post',
                data:{"carType":"11"}
            }).then(function (response) {
                if (response.status <= 200) {
                    res.form = response.data;
                    console.log(response.data);
                }
            }).catch(function (error) {
                res.$message.error(error);
                console.log(error);
            });
        },
        methods: {
            onSubmit() {
                var res = this;
                this.$axios({
                    url: 'JinshiParkSetting/updateJinshiParkSetting',
                    method: 'post',
                    data:{"carType":res.activeIndex,"settingForm":res.form}
                }).then(function (response) {
                    if (response.status <= 200) {
                        console.log(res.activeIndex);
                        res.$message.success('修改成功！');
                    }
                }).catch(function (error) {
                    res.$message.error('修改失败： '+error);
                    console.log(error);
                });
                
            },
            handleSelect(key, keyPath) {
                var res = this;
                if(key==11){
                    this.activeIndex = 11;
                    this.titleName = "小型车";
                    this.$axios({
                        url: 'JinshiParkSetting/getGlobalVariable',
                        method: 'post',
                        data:{"carType":key}
                    }).then(function (response) {
                        if (response.status <= 200) {
                            res.form = response.data;
                            console.log(response.data);
                        }
                    }).catch(function (error) {
                        res.$message.error(error);
                        console.log(error);
                    });
                }else if(key==12){
                    this.activeIndex = 12;
                    this.titleName = "中型车(黄牌)";
                    this.$axios({
                        url: 'JinshiParkSetting/getGlobalVariable',
                        method: 'post',
                        data:{"carType":key}
                    }).then(function (response) {
                        if (response.status <= 200) {
                            res.form = response.data;
                            console.log(response.data);
                        }
                    }).catch(function (error) {
                        res.$message.error(error);
                        console.log(error);
                    });
                }else if(key==13){
                    this.activeIndex = 13;
                    this.titleName = "大型车(黄牌)";
                    this.$axios({
                        url: 'JinshiParkSetting/getGlobalVariable',
                        method: 'post',
                        data:{"carType":key}
                    }).then(function (response) {
                        if (response.status <= 200) {
                            res.form = response.data;
                            console.log(response.data);
                        }
                    }).catch(function (error) {
                        res.$message.error(error);
                        console.log(error);
                    });
                }else if(key==14){
                    this.activeIndex = 14;
                    this.titleName = "新能源车";
                    this.$axios({
                        url: 'JinshiParkSetting/getGlobalVariable',
                        method: 'post',
                        data:{"carType":key}
                    }).then(function (response) {
                        if (response.status <= 200) {
                            res.form = response.data;
                            console.log(response.data);
                        }
                    }).catch(function (error) {
                        res.$message.error(error);
                        console.log(error);
                    });
                }else if(key==15){
                    this.activeIndex = 15;
                    this.titleName = "特种车";
                    this.$axios({
                        url: 'JinshiParkSetting/getGlobalVariable',
                        method: 'post',
                        data:{"carType":key}
                    }).then(function (response) {
                        if (response.status <= 200) {
                            res.form = response.data;
                            console.log(response.data);
                        }
                    }).catch(function (error) {
                        res.$message.error(error);
                        console.log(error);
                    });
                }
            }
        }
    }
</script>