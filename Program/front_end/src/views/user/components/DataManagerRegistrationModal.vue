<template>
    <a-modal
        :visible="DataManagerRegistrationModalVisible"
        title="添加数据操作员"
        cancelText="取消"
        okText="确定"
        @cancel="cancel"
        @ok="handleSubmit"
    >
        <a-Form :form="form">
            <a-form-item v-bind="formItemLayout" label="用户邮箱">
                <a-input
                    v-decorator="[
                        'email',
                        { rules: [{required: true, message: '请输入用户邮箱', }], validateTrigger: 'blur' }
                    ]"
                >
                    <a-icon slot="prefix" type="mail" :style="{ color: 'rgba(0,0,0,.25)' }"/>
                </a-input>
            </a-form-item>
            <a-form-item v-bind="formItemLayout" label="用户名">
                <a-input
                    v-decorator="[
                        'userName',
                        { rules: [{required: true, message: '请输入用户名', }] , validateTrigger: 'blur' }
                    ]"
                >
                    <a-icon slot="prefix" type="user" :style="{ color: 'rgba(0,0,0,.25)' }"/>
                </a-input>
            </a-form-item >
            <a-form-item v-bind="formItemLayout" label="手机号">
                <a-input
                    v-decorator="[
                        'phoneNumber',
                        { rules: [{required: true, message: '请输入手机号', }], validateTrigger: 'blur' }
                    ]"
                >
                    <a-icon slot="prefix" type="phone" :style="{ color: 'rgba(0,0,0,.25)' }"/>
                </a-input>
            </a-form-item>
            <a-form-item v-bind="formItemLayout" label="密码">
                <a-input
                    type="password"
                    v-decorator="[
                        'password',
                        { rules: [{required: true, message: '请输入密码', }, { validator: this.handlePassword }], validateTrigger: 'blur' }
                    ]"
                >
                    <a-icon slot="prefix" type="lock" :style="{ color: 'rgba(0,0,0,.25)' }"/>
                </a-input>
            </a-form-item >        
            <a-form-item v-bind="formItemLayout" label="确认密码">
                <a-input
                    type="password"
                    v-decorator="[
                        'passwordConfirm',
                        { rules: [{required: true, message: '请输入密码', }, { validator: this.handlePasswordCheck }], validateTrigger: 'blur'}
                    ]"
                >
                    <a-icon slot="prefix" type="lock" :style="{ color: 'rgba(0,0,0,.25)' }"/>
                </a-input>
            </a-form-item >   
        </a-Form>
    </a-modal>
</template>

<script>
import { mapGetters, mapMutations, mapActions } from 'vuex'
export default {
    name: 'DataManagerRegistrationModal',
    data() {
        return {
            formItemLayout: {
                labelCol: {
                    xs: { span: 12 },
                    sm: { span: 6 },
                },
                wrapperCol: {
                    xs: { span: 24 },
                    sm: { span: 16 },
                },
            },
        }
    },
    computed: {
        ...mapGetters([
            'DataManagerRegistrationModalVisible',
        ])
    },
    beforeCreate() {
        this.form = this.$form.createForm(this, { name: 'DataManagerRegistrationModal' });
    },
    mounted() {

    },
    methods: {
        ...mapMutations([
            'set_DataManagerRegistrationModalVisible',
        ]),
        ...mapActions([
            'registerDataManager',
        ]),

        handlePassword(rule, value, callback) {
            if (value.length < 6) {
                callback(new Error('密码长度至少6位'))
            }
            callback()
        },
        handlePasswordCheck(rule, value, callback) {
            const password = this.form.getFieldValue('password')
            console.log(password)
            // if (value === undefined) {
            //     callback(new Error('请输入密码'))
            // }
            if (value && password && value.trim() !== password.trim()) {
                callback(new Error('两次密码不一致'))
            }
            callback()
        },
        cancel() {
            this.set_DataManagerRegistrationModalVisible(false)
        },
        handleSubmit(e) {
            e.preventDefault();
            this.form.validateFieldsAndScroll((err, values) => {
                if (!err) {
                    const data = {
                        email: this.form.getFieldValue('email'),
                        username: this.form.getFieldValue('userName'),
                        phoneNumber: this.form.getFieldValue('phoneNumber'),
                        password: this.form.getFieldValue('password')
                    }
                    this.registerDataManager(data)
                }
            });
        },
    }
}
</script>