<template>
  <el-cascader
    v-model="cascader"
    :options="options"
    @change="handleChange"
    placeholder="请选择团队和项目"
    size="small"
    clearable
  />
</template>

<script>
export default {
  data () {
    return {
      cascader: [],
      options: []
    }
  },
  mounted () {
    this.getCascader()
  },
  methods: {
    getCascader () {
      this.$axios
        .post('/api/v1/team/aggregate', { cascader: null })
        .then((res) => {
          this.options = res.data.data
        }).catch((error) => {
          this.$message({
            type: 'error',
            message: error.response.data.message,
            center: true
          })
        })
    },
    handleChange (value) {
      this.cascader = value
      const data = {
        team: value[0],
        project: value[1]
      }
      this.$emit('selectedTeamProject', data)
    }
  }
}
</script>
