<template>
    <div>
        <span class="text-blue-500 text-base cursor-pointer" v-has="'user:edit'">
            <i class="fa fa-fw fa-pencil" @click = "edit"></i>
        </span>
        <el-dropdown placement="bottom" class ="dropdown">
              <span class="text-blue-500 text-base">
                  <i class="fa fa-fw fa-ellipsis-h"></i>
              </span>
            <el-dropdown-menu slot="dropdown">
                <template v-for = "{popover,action,popText,label,permission} in dropDownItem">
                    <template v-if = "popover">
                        <popover-confirm @confirm = "confirm(action)" v-if = "hasPermission(permission)">
                            <div slot="popover-title">
                                <span>{{popText}}</span>
                            </div>
                            <div slot="popover-content">
                                <el-dropdown-item>{{label}}</el-dropdown-item>
                            </div>
                        </popover-confirm>
                    </template>
                    <template v-else>
                        <el-dropdown-item @click.native = "action" v-if = "hasPermission(permission)">
                            {{label}}
                        </el-dropdown-item>
                    </template>
                </template>
            </el-dropdown-menu>
        </el-dropdown>
    </div>
</template>

<script>
    import PopoverConfirm from '@/components/popoverConfirm'
    import {mapState} from 'vuex'

    export default {
        name: "PopDropdown",
        components : {
            PopoverConfirm
        },
        data(){
            return {
                dropDownItem: [
                    {
                        label: this.$t('common_read'),
                        icon: '',
                        action: this.handleDetail,
                        popover: false,
                        permission : 'user:detail'
                    },
                    {
                        label: this.$t('sys_user_pwd'),
                        icon: '',
                        action: this.handlePwd,
                        popover: false,
                        permission : 'user:password'
                    },
                    {
                        label: this.$t('common_delete'),
                        icon: '',
                        action: this.handleDel,
                        popover: true,
                        popText : this.$t('common_confirm_do'),
                        permission : 'user:delete'
                    },
                    {
                        label: this.$t('sys_user_frozen'),
                        icon: '',
                        action: this.frozen,
                        popover: true,
                        popText : this.$t('common_confirm_do'),
                        permission : 'user:frozen'
                    },
                    {
                        label: this.$t('sys_user_proxy_man'),
                        icon: '',
                        action: this.proxyMan,
                        popover: false,
                        permission : 'user:proxyMan'
                    },
                ],
            }
        },
        computed: {
            ...mapState({
                allAuth: ({user}) => user.allAuth
            })
        },
        methods : {
            hasPermission(permission) {
                let findPermission = this.allAuth.find(item => item.action === permission)
                return findPermission && findPermission.type !== '2'
            },
            edit(){
                this.$emit('edit')
            },
            handleDetail(){
                this.$emit('handleDetail')
            },
            handlePwd(){
                this.$emit('handlePwd')
            },
            handleDel(){
                this.$emit('handleDel')
            },
            frozen(){
                this.$emit('frozen')
            },
            proxyMan(){
                this.$emit('proxyMan')
            },
            confirm(event){
               event()
            }
        }
    }
</script>

<style scoped>

</style>