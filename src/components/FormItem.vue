<template>
    <div>
        <label for="">{{label}}</label>
        <div>
            <slot></slot>
            <p v-if="errStatus">{{errMessage}}</p>
        </div>
    </div>
</template>
<script>
    import Schema from 'async-validator'
    export default{
        inject: ['KForm'],
        props:['label','prop'],
        data(){
            return{
                errMessage: '',
                errStatus: false
            }
        },
        mounted(){
            this.$on('validate',this.validator)
        },
        methods:{
            validator(){
                //有两个Input，一个用户名，一个密码
                const rules = this.KForm.rules[this.prop];
                const value = this.KForm.model[this.prop];

                const descriptor = {[this.prop]:rules};
                // console.log('开始校验了')
                const schema = new Schema({descriptor});
                schema.validate({[this.prop]:value},errors =>{
                    if(errors){
                        this.errMessage = errors[0].message;
                        this.errStatus = true;
                    }else{
                        this.errMessage = '';
                        this.errStatus = '';
                    }
                })
            }
        }
    }
</script>
<style scoped>

</style>