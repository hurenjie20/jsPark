<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i> 车场设置</el-breadcrumb-item>
                <el-breadcrumb-item>车场设置</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="form-box">
                <el-form ref="form" :model="form" label-width="120px">
                    <el-form-item label="车场名称： ">
                        <el-input v-model="form.jpName"></el-input>
                    </el-form-item>
                    <el-form-item label="车场编号： ">
                        <el-input v-model="form.jpNumber"></el-input>
                    </el-form-item>
                    <el-form-item label="车场地址：">
                        <el-input v-model="form.jpSite"></el-input>
                    </el-form-item>
                    <el-form-item label="联系人姓名：">
                        <el-input v-model="form.jpMembers"></el-input>
                    </el-form-item>
                    <el-form-item label="联系人手机号：">
                        <el-input v-model="form.jpPhoneNumber"></el-input>
                    </el-form-item>
                    <el-form-item label="车 位 数 ：">
                        <el-input v-model="form.jpPlaceNumber"></el-input>
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
                    jpId: '',
                    jpName: '',
                    jpNumber: '',
                    jpSite: '',
                    jpMembers: '',
                    jpPhoneNumber: '',
                    jpPlaceNumber: ''
                }
            }
        },
        methods: {
            onSubmit() {
                var res = this;
                this.$axios({
                    url: 'jinshiParking/updateByPrimaryKey',
                    method: 'post',
                    data:{"parkSetting":res.form}
                }).then(function (response) {
                    if (response.status <= 200) {
                        res.$message.success('修改成功！');
                    }
                }).catch(function (error) {
                    res.$message.error('修改失败！');
                    console.log(error);
                });
            }
        },
        created(){
            var res = this;
            this.$axios({
                url: 'jinshiParking/selectByNumber',
                method: 'post'
            }).then(function (response) {
                if (response.status <= 200) {
                    console.log(response.data)
                    res.form = response.data;
                }
            }).catch(function (error) {
                res.$message.error('查询失败！');
                console.log(error);
            });
        }
    }
</script>