<template>
  <div>
    <Message :show.sync="msgShow" :type="msgType" :msg="msg"/>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      msg: '', // 消息
      msgType: '', // 消息类型
      msgShow: false // 是否显示消息，默认不显示
    }
  },
  beforeRouteEnter(to, from, next) {
    const fromName = from.name
    // 获取 logout 参数
    const logout = to.params.logout

    next(vm => {
      if (vm.$store.state.auth) {
        switch (fromName) {
          case 'Register':
            vm.showMsg('注册成功')
            break
          // 已登录时，从登录页面跳转过来
          case 'Login':
            // 显示登录成功
            vm.showMsg('登录成功')
            break
        }
      } else if (logout) {
        // logout 返回 true 时，显示操作成功提示
        vm.showMsg('操作成功')
      }
    })
  },
  computed: {
    // 用户登录状态
    auth() {
      return this.$store.state.auth
    }
  },
  watch: {
    // 监听 auth，它的值变为 false 时，显示操作成功提示
    auth(value) {
      if (!value) {
        this.showMsg('操作成功')
      }
    }
  },
  methods: {
    showMsg(msg, type = 'success') {
      this.msg = msg
      this.msgType = type
      this.msgShow = true
    }
  }
}
</script>

<style scoped>

</style>