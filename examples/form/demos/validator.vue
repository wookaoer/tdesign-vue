<template>
  <div>
    <t-form
      :data="formData"
      :rules="rules"
      ref="form"
      @reset="onReset"
      @submit="onSubmit"
      scrollToFirstError="smooth"
    >
      <t-form-item label="用户名" help="这里请填写用户名" name='account'>
        <t-input v-model="formData.account"></t-input>
      </t-form-item>
      <t-form-item label="密码" help="这里请填写密码" name='password'>
        <t-input type="password" v-model="formData.password"></t-input>
      </t-form-item>
      <t-form-item label="邮箱" name='email'>
        <t-input v-model="formData.email"></t-input>
      </t-form-item>
      <t-form-item label="性别" name='gender'>
        <t-radio-group v-model="formData.gender">
          <t-radio value="male">男</t-radio>
          <t-radio value="femal">女</t-radio>
        </t-radio-group>
      </t-form-item>
      <t-form-item label="课程" name='course'>
        <t-checkbox-group
          v-model="formData.course"
          :options="courseOptions"
        ></t-checkbox-group>
      </t-form-item>
      <t-form-item label="学院" name='college'>
        <t-select v-model="formData.college" class="demo-select-base" clearable>
          <t-option v-for="(item, index) in options" :value="item.value" :label="item.label" :key="index">
            {{ item.label }}
          </t-option>
        </t-select>
      </t-form-item>
      <t-form-item label="入学时间" name='date' :rules="[{ date: { delimiters: ['/', '-', '.'] }, message: '日期格式有误' }]">
        <t-input v-model="formData.date"></t-input>
      </t-form-item>
      <t-form-item label="个人网站" name='content.url'>
        <t-input v-model="formData.content.url"></t-input>
      </t-form-item>
      <t-form-item style="padding-top: 8px">
        <t-button theme="primary" type="submit" style="margin-right: 10px">提交</t-button>
        <t-button theme="default" variant="base" type="reset" style="margin-right: 10px">重置</t-button>
        <t-button theme="default" variant="base" @click="handleClear">清除校验结果</t-button>
      </t-form-item>
    </t-form>
  </div>
</template>
<script>

const INITIAL_DATA = {
  account: '',
  password: '',
  email: '',
  gender: '',
  college: '',
  date: '',
  content: {
    url: '',
  },
  course: [],
};
export default {
  data() {
    return {
      formData: { ...INITIAL_DATA },
      courseOptions: [
        { label: '语文', value: '1' },
        { label: '数学', value: '2' },
        { label: '英语', value: '3' },
        { label: '体育', value: '4' },
      ],
      options: [
        { label: '计算机学院', value: '1' },
        { label: '软件学院', value: '2' },
        { label: '物联网学院', value: '3' },
      ],
      rules: {
        account: [
          { required: true, message: '姓名必填', type: 'error' },
          { min: 2, message: '至少需要两个字', type: 'error' },
        ],
        password: [
          { required: true, message: '密码必填', type: 'error' },
        ],
        email: [
          { required: true, message: '格式必须为邮箱', type: 'warning' },
        ],
        gender: [
          { required: true, message: '性别必填', type: 'warning' },
        ],
        course: [
          { required: true, message: '课程必填', type: 'warning' },
        ],
        'content.url': [
          { required: true, message: '个人网站必填', type: 'warning' },
          {
            url: {
              protocols: ['http', 'https', 'ftp'],
              require_protocol: true,
            },
            message: '请输入正确的个人主页',
          },
        ],
      },
    };
  },

  methods: {
    onReset() {
      this.$message.success('重置成功');
      console.log('formData', this.formData);
    },
    onSubmit({ validateResult, firstError }) {
      if (validateResult === true) {
        this.$message.success('提交成功');
      } else {
        console.log('Errors: ', validateResult);
        this.$message.warning(firstError);
      }
    },
    handleClear() {
      this.$refs.form.clearValidate();
    },
  },
};
</script>

<style scoped>
.demo-select-base {
  width: 300px;
}
</style>
