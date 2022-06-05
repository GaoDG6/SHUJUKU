<template>
  <div>
    <el-form>
      <el-row :gutter="40">
        <el-col :span="12">
          <div class="grid-content bg-purple">
            <el-form-item label="姓名">
              <el-input v-model="form.name" :disabled="true"> </el-input>
            </el-form-item>
            <el-form-item label="性别">
              <el-input v-model="form.sex" :disabled="true"> </el-input>
            </el-form-item>
            <el-form-item label="专业">
              <el-input v-model="form.major" :disabled="true"> </el-input>
            </el-form-item>
            <el-form-item label="寝室ID">
              <el-input v-model="form.dormId" :disabled="true"> </el-input>
            </el-form-item>
          </div>
        </el-col>
        <el-col :span="12">
          <div class="grid-content bg-purple">
            <el-form-item label="学号">
              <el-input v-model="form.sId" :disabled="true"> </el-input>
            </el-form-item>
            <el-form-item label="联系方式">
              <el-input v-model="form.phone" :disabled="true"> </el-input>
            </el-form-item>
            <el-form-item label="密码">
              <el-input v-model="form.password" :disabled="true"> </el-input>
            </el-form-item>
            <el-button type="info" @click="dialogFormVisible = true">修改密码和联系方式</el-button>
            <el-dialog title="修改密码和联系方式" :visible.sync="dialogFormVisible" width="30%">
              <el-form :model="form">
                <el-form-item label="密码">
                  <el-input v-model="form.password" :disabled="true"> </el-input>
                </el-form-item>
                <el-form-item label="密码">
                  <el-input v-model="form.password"> </el-input>
                </el-form-item>
              </el-form>
              <el-form :model="form">
                <el-form-item label="联系方式">
                  <el-input v-model="form.phone" :disabled="true"> </el-input>
                </el-form-item>
                <el-form-item label="联系方式">
                  <el-input v-model="form.phone"> </el-input>
                </el-form-item>
              </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="
                  dialogFormVisible = false;
                onsure();
                ">确 定</el-button>
              </div>
            </el-dialog>
            
          </div>
        </el-col>

      </el-row>
    </el-form>
    <h1>寝室卫生评比</h1>
    <el-table :data="cleanData" style="width: 100%">
      <el-table-column prop="date" label="评比时间" width="180">
      </el-table-column>
      <el-table-column prop="dormId" label="寝室ID" width="180">
      </el-table-column>
      <el-table-column prop="aveScore" label="平均得分" width="180">
      </el-table-column>
      <el-table-column prop="rank" label="卫生等级" width="180">
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
export default {
  data() {
    return {
      tokenStr:"",
      dialogFormVisible: false,
      form: {
        sId: "20051000",
        sex: "男",
        name: "111",
        major: "计算机",
        phone: "1234567890",
        dormId: "12345",
        password: "222222222",
      },
      cleanData: [{
        date: "",
        dormId: '12345',
        aveScore: '99',
        rank: 'A'
      }],
    };
  },
  created() {
this.tokenStr = JSON.parse(window.sessionStorage.getItem("token"));
this.http.get("/user/queryStudent",{
          headers: { Authorization: this.tokenStr },
        })
  },
  methods: {
    // 修改密码
    onsure() {

    }

  },
};
</script>
<style lang="less" scoped>
</style>