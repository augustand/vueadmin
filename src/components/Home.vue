<template>
    <div class="ui tiny teal progress" v-show="isShow">
        <div class="bar" style="transition-duration: 300ms;min-width:0;height:3px;" :style="{width: width + '%'}"></div>
    </div>
    <div class="home-header">
        <a href="https://github.com/rootsli/vueadmin" target="_blank">
            <img src="../assets/img/logo2.png" class="logo" alt="logo" />
        </a>
        <div class="ui inline pointing dropdown">
            <div class="text">
                <img class="ui avatar image" src="../assets/img/avatar_small.jpg"> 张小泉
                <i class="dropdown icon"></i>
            </div>
            <div class="menu ui transition">
                <a class="item" href="http://www.yeelink.net"><i class="reply mail icon"></i>返回首页</a>
                <a class="item" href="javascript:;" @click="logout"><i class="sign out icon"></i>注销登录</a>
            </div>
        </div>
    </div>
    <div class="home-main">
            <!-- menu -->
        <div class="main-menu">
            <menu></menu>
        </div>
        <div class="main-content">
            <div class="menu-toggle" @click="toggleMenu">{{toggleTitle}}</div>
            <!-- page container -->
            <router-view></router-view>
        </div>
    </div>
</template>
<script>
    import Menu from './menu/Menu'
    import {setProgress} from '../vuex/actions'

    export default{
        data(){
            return {
                width: 0,
                isShow: false,
                toggleTitle: '隐藏菜单'
            }
        },
        vuex: {
            getters: {
                progress: ({progress}) => progress.rate || 0
            },
            actions: {
                setProgress
            }
        },
        watch: {
            'progress': {
                deep: true,
                handler: function (val) {
                    if (val == 1) { //有路由切换
                        this.isShow = true;
                        this.width = 10
                        setTimeout(()=> {
                            this.width = 60
                            setTimeout(()=> {
                                this.width = 100
                                setTimeout(()=> {
                                    this.isShow = false;
                                    this.width = 0
                                    this.setProgress(0)
                                }, 300)
                            }, 200)
                        }, 100)
                    }
                }
            }
        },
        components: {
            Menu
        },
        methods: {
            toggleMenu(){
                $(".main-menu").toggle(300)
                if (this.toggleTitle == '隐藏菜单') {
                    $(".main-content").animate({
                        left: '0px'
                    }, 200)
                    this.toggleTitle = '显示菜单'
                } else {
                    $(".main-content").animate({
                        left: '200px'
                    }, 200)
                    this.toggleTitle = '隐藏菜单'
                }
            },
            logout(){
                this.$route.router.go('/')
            }
        },
        ready(){
            $('.home-header .ui.dropdown').dropdown({on: 'hover'})
        }
    }
</script>
<style scoped>
    .ui.inline.dropdown {
        float: right;
        margin-right: 35px;
    }

    .ui.inline.dropdown:hover {
        background-color: #53575E;
    }

    .ui.pointing.dropdown > .menu {
        margin-top: 0 !important;
    }

    .main-menu .breadcrumb-item {
        height: 39px;
        line-height: 39px;
        background-color: #FFFFFF;
        padding-left: 12px;
    }

    .home-header {
        overflow: visible;
        right: 0;
        z-index: 500;
        min-width: 900px;
        height: 44px;
        line-height: 44px;
        color: #fff;
        background-color: #2a2d34;
    }

    .home-main {
        position: absolute;
        top: 44px;
        right: 0;
        bottom: 0;
        left: 0;
        min-width: 900px;
        font-size: 14px;
    }

    .home-main .main-menu {
        width: 200px;
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        z-index: 2;
        margin-bottom: 0;
        border-right: 3px solid #0cadb7;
        background-color: #293541;
        float: left;
        height: 100%;
        color: #bbb;
        overflow: hidden;
    }

    .home-main .main-content {
        float: left;
        height: 100%;
        overflow-x: auto;
        overflow-y: hidden;
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 200px;
    }

    .ui.progress {
        position: absolute !important;
        width: 100%;
    }

    .menu-toggle {
        float: left;
        background-color: #0cadb7;
        width: 20px;
        margin-top: 410px;
        font-size: 14px;
        color: #fff;
        padding: 2px;
        cursor: pointer;
        border-top-right-radius: 6px;
        border-bottom-right-radius: 6px;
    }

    .menu-toggle:hover {
        background-color: rgba(12, 173, 183, 0.76);
    }

    .logo{
        vertical-align: middle;
    }
</style>
<style>
    .page-container {
        height: 100%;
        width: 100%;
        padding: 22px;
    }
    .ui.table a.operator:not(:first-child){
        margin-left: 12px !important;
    }
</style>
