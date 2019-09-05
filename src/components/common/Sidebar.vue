<template>
    <div class="sidebar">
        <el-menu class="sidebar-el-menu" :default-active="onRoutes" :collapse="collapse" background-color="#324157"
            text-color="#bfcbd9" active-text-color="#20a0ff" unique-opened router>
            <template v-for="item in items">
                <template v-if="item.subs">
                    <el-submenu :index="item.index" :key="item.index">
                        <template slot="title">
                            <i :class="item.icon"></i><span slot="title">{{ item.title }}</span>
                        </template>
                        <template v-for="subItem in item.subs">
                            <el-submenu v-if="subItem.subs" :index="subItem.index" :key="subItem.index">
                                <template slot="title">{{ subItem.title }}</template>
                                <el-menu-item v-for="(threeItem,i) in subItem.subs" :key="i" :index="threeItem.index">
                                    {{ threeItem.title }}
                                </el-menu-item>
                            </el-submenu>
                            <el-menu-item v-else :index="subItem.index" :key="subItem.index">
                                {{ subItem.title }}
                            </el-menu-item>
                        </template>
                    </el-submenu>
                </template>
                <template v-else>
                    <el-menu-item :index="item.index" :key="item.index">
                        <i :class="item.icon"></i><span slot="title">{{ item.title }}</span>
                    </el-menu-item>
                </template>
            </template>
        </el-menu>
    </div>
</template>

<script>
    import bus from '../common/bus';
    export default {
        data() {
            return {
                collapse: false,
                items: [
                    {
                        icon: 'el-icon-lx-home',
                        index: 'dashboard',
                        title: '总览'
                    },
                    {
                        icon: 'el-icon-lx-cascades',
                        index: 'outInControl',
                        title: '出入管理'
                    },
                    {
                        icon: 'el-icon-lx-cascades',
                        index: 'carManager',
                        title: '车辆管理'
                    },
                    {
                        icon: 'el-icon-lx-cascades',
                        index: 'financialManagement',
                        title: '财务管理'
                    },
                    // {
                    //     icon: 'el-icon-lx-cascades',
                    //     index: 'inOutRecord',
                    //     title: '进出记录'
                    // },
                    {
                        icon: 'el-icon-lx-cascades',
                        index: '2',
                        title: '记录管理',
                        subs: [
                            {
                                index: 'goin',
                                title: '出场记录'
                            },
                            {
                                index: 'goout',
                                title: '入场记录'
                            }
                        ]
                    },
                    {
                        icon: 'el-icon-lx-cascades',
                        index: 'serviceManagement',
                        title: '服务管理'
                    },
                    {
                        icon: 'el-icon-lx-cascades',
                        index: 'whiteListManager',
                        title: '黑/白名单'
                    },
                    {
                        icon: 'el-icon-lx-cascades',
                        index: 'yardManager',
                        title: '车场管理'
                    },
                    {
                        icon: 'el-icon-lx-cascades',
                        index: 'agentManagement',
                        title: '代理商管理'
                    },

                    {
                        icon: 'el-icon-lx-cascades',
                        index: 'companyManagement',
                        title: '公司管理'
                    },
                    {
                        icon: 'el-icon-lx-cascades',
                        index: 'departmentManagement',
                        title: '部门管理'
                    },
                    {
                        icon: 'el-icon-lx-calendar',
                        index: '3',
                        title: '设置',
                        subs: [
                            {
                                index: 'parkingSetting',
                                title: '车场设置'
                            },
                            {
                                index: 'camerasSetting',
                                title: '摄像机设置'
                            },
                            {
                                index: 'parkingMode',
                                title: '模式',
                            },
                            {
                                index: 'parkingCharge',
                                title: '收费设置'
                            }
                        ]
                    },
                    {
                        icon: 'el-icon-lx-cascades',
                        index: 'table8',
                        title: '日志'
                    },
                    {
                        icon: 'el-icon-lx-cascades',
                        title: '退出'
                    },
                    // {
                    //     icon: 'el-icon-lx-copy',
                    //     index: 'tabs',
                    //     title: 'tab选项卡'
                    // },
                    // {
                    //     icon: 'el-icon-lx-calendar',
                    //     index: '3',
                    //     title: '表单相关',
                    //     subs: [
                    //         {
                    //             index: 'form',
                    //             title: '基本表单'
                    //         },
                    //         {
                    //             index: '3-2',
                    //             title: '三级菜单',
                    //             subs: [
                    //                 {
                    //                     index: 'editor',
                    //                     title: '富文本编辑器'
                    //                 },
                    //                 {
                    //                     index: 'markdown',
                    //                     title: 'markdown编辑器'
                    //                 },
                    //             ]
                    //         },
                    //         {
                    //             index: 'upload',
                    //             title: '文件上传'
                    //         }
                    //     ]
                    // },
                    // {
                    //     icon: 'el-icon-lx-emoji',
                    //     index: 'icon',
                    //     title: '自定义图标'
                    // },
                    // {
                    //     icon: 'el-icon-pie-chart',
                    //     index: 'charts',
                    //     title: 'schart图表'
                    // },
                    // {
                    //     icon: 'el-icon-rank',
                    //     index: '6',
                    //     title: '拖拽组件',
                    //     subs: [
                    //         {
                    //             index: 'drag',
                    //             title: '拖拽列表',
                    //         },
                    //         {
                    //             index: 'dialog',
                    //             title: '拖拽弹框',
                    //         }
                    //     ]
                    // },
                    // {
                    //     icon: 'el-icon-lx-global',
                    //     index: 'i18n',
                    //     title: '国际化功能'
                    // },
                    // {
                    //     icon: 'el-icon-lx-warn',
                    //     index: '7',
                    //     title: '错误处理',
                    //     subs: [
                    //         {
                    //             index: 'permission',
                    //             title: '权限测试'
                    //         },
                    //         {
                    //             index: '404',
                    //             title: '404页面'
                    //         }
                    //     ]
                    // }
                    // ,
                    // {
                    //     icon: 'el-icon-lx-redpacket_fill',
                    //     index: '/donate',
                    //     title: '支持作者'
                    // }
                ]
            }
        },
        computed:{
            onRoutes(){
                return this.$route.path.replace('/','');
            }
        },
        created(){
            // 通过 Event Bus 进行组件间通信，来折叠侧边栏
            bus.$on('collapse', msg => {
                this.collapse = msg;
            })
        }
    }
</script>

<style scoped>
    .sidebar{
        display: block;
        position: absolute;
        left: 0;
        top: 70px;
        bottom:0;
        overflow-y: scroll;
    }
    .sidebar::-webkit-scrollbar{
        width: 0;
    }
    .sidebar-el-menu:not(.el-menu--collapse){
        width: 250px;
    }
    .sidebar > ul {
        height:100%;
    }
</style>
