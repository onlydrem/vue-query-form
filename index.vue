<template>
  <div class="query-form-panel">
    <el-form ref="ruleForm"
             :model="ruleForm"
             status-icon
             class="demo-ruleForm"
             label-width="100px">
      <el-row :gutter="24"
              style="width: 85%">
        <div v-for="(item, index) in fields"
             :key="item.id">
          <el-col class="form-item"
                  :xs="24"
                  :sm="12"
                  :md="12"
                  :lg="8">
            <el-form-item :label="item.fieldName"
                          :prop="item.fieldCode"
                          v-if="!showMore ? index <= 2 : index >= 0">
              <el-input clearable
                        v-if="item.fieldType == 'INPUT'"
                        v-model.trim="ruleForm[item.fieldCode]"
                        type="text"
                        autocomplete="off"
                        placeholder="请输入" />
              <template v-if="item.fieldType == 'DOUBLE_INPUT'">
                <slot />
              </template>
              <el-date-picker v-if="item.fieldType == 'DATE_TIME_RANGE'"
                              v-model="ruleForm[item.fieldCode]"
                              type="datetimerange"
                              range-separator="至"
                              start-placeholder="开始日期"
                              end-placeholder="结束日期"
                              align="right"
                              style="width: 100%"
                              value-format="yyyy-MM-dd">
              </el-date-picker>
              <el-date-picker v-if="item.fieldType == 'DATE_RANGE'"
                              v-model="ruleForm[item.fieldCode]"
                              type="daterange"
                              range-separator="至"
                              start-placeholder="开始日期"
                              end-placeholder="结束日期"
                              align="right"
                              style="width: 100%"
                              value-format="yyyy-MM-dd">
              </el-date-picker>
              <el-select clearable
                         v-if="item.fieldType == 'SELECT'"
                         v-model="ruleForm[item.fieldCode]"
                         placeholder="请选择状态"
                         style="width: 100%">
                <el-option v-for="item in item.selectOptions"
                           :key="item.value"
                           :label="item.label"
                           :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </div>
      </el-row>
      <div class="right-content">
        <div class="btn-group">
          <el-button type="primary"
                     @click="submitForm">查询</el-button>
          <el-button @click="resetQuery">重置</el-button>
          <el-button type="text"
                     class="show-more"
                     @click="handleClickShowMore"
                     v-if="fields.length > 3">
            {{ showMore ? "收起" : "展开" }}
            <i :class="showMore ? 'el-icon-arrow-up' : 'el-icon-arrow-down'"></i>
          </el-button>

        </div>
      </div>
    </el-form>
  </div>
</template>

<script>
export default {
  mixins: [],
  data () {
    return {
      showMore: false
    };
  },
  props: ["ruleForm", "fields"],
  components: {},
  beforeRouteEnter (to, from, next) {
    next();
  },
  beforeRouteLeave (to, from, next) {
    next();
  },
  created () { },
  mounted () { },
  updated () { },
  destroyed () { },
  methods: {
    handleClickShowMore () {
      this.showMore = !this.showMore;
    },
    submitForm () {
      this.$emit("submitForm", this.ruleForm);
    },
    // 重置
    resetQuery () {
      this.resetForm("ruleForm");
      console.log(this.ruleForm, '333')
      this.$emit("submitForm", {});
    }
  },
  computed: {},
  filters: {}
};
</script>

<style scoped lang="scss">
@import "@as/styles/main.scss";
.demo-ruleForm {
  @include flex();
  -webkit-box-pack: justify;
  -webkit-justify-content: space-between;
  justify-content: space-between;
}
.el-input {
  display: block !important;
  height: 32px !important;
  line-height: 32px !important;
}
.el-select {
  display: block !important;
  height: 32px !important;
  line-height: 32px !important;
}
.el-date-editor {
  height: 40px !important;
  line-height: 40px !important;
}
::v-deep .el-range-editor--medium .el-range-input {
  font-size: 14px !important;
  height: 28px !important;
  line-height: 28px !important;
}
.form-item {
  margin-bottom: 5px;
}

.right-content {
  min-width: 200px;
  .btn-group {
    @include clearfix();
    float: right;
    margin-left: 5px;
    .show-more {
      color: #00b39b;
      font-size: 14px;
      margin-left: 5px;
    }
  }
}
</style>
