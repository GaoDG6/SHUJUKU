<template>
  <div>
    <el-container>
      <!-- 水电费 -->
      <el-aside width="580x">
        <h2>当前水电费余额</h2>
        <el-table :data="dorm" style="width: 100%">
          <el-table-column prop="dormId" label="寝室ID" width="180">
          </el-table-column>
          <el-table-column prop="waterBalance" label="水费余额" width="180">
          </el-table-column>
          <el-table-column prop="eletricBalance" label="电费余额" width="180">
          </el-table-column>
        </el-table>
        <br>
        <!-- 缴费 -->
        <el-row :gutter="0">
          <el-col :span="4" :offset="20">
            <div>
              <el-button type="primary" @click="dialogChargeFormVisible = true">缴费</el-button>
              <!-- 弹窗 -->
              <el-dialog title="缴费" :visible.sync="dialogChargeFormVisible">
                <el-form :model="chargeForm">
                  <el-row :gutter="40">
                    <el-col :span="8">
                      <div></div>
                      <el-form-item label="学号">
                        <el-input v-model="chargeForm.sId"> </el-input>
                      </el-form-item>
                      <el-form-item label="寝室编号">
                        <el-input v-model="chargeForm.dormId"> </el-input>
                      </el-form-item>
                    </el-col>
                    <el-col :span="8">
                      <div>
                        <el-form-item label="缴费类型">
                          <el-input v-model="chargeForm.chargeType"> </el-input>
                        </el-form-item>
                        <el-form-item label="支付方式">
                          <el-input v-model="chargeForm.payType"> </el-input>
                        </el-form-item>
                      </div>
                    </el-col>
                    <el-col :span="8">
                      <div>
                        <el-form-item label="金额">
                          <el-input v-model="chargeForm.money"> </el-input>
                        </el-form-item>
                      </div>
                    </el-col>
                  </el-row>
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
        <h2>缴费记录</h2>
        <el-table height:250px :data="transaction" style="width: 100%">
          <el-table-column label="缴费日期" width="180">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.chargeTime }}</span>
            </template>
          </el-table-column>
          <el-table-column label="缴费类型" width="180">
            <template slot-scope="scope">
              <el-popover trigger="hover" placement="top">
                <p>缴费学生学号: {{ scope.row.sId }}</p>
                <p>缴费宿舍: {{ scope.row.dormId }}</p>
                <p>支付方式: {{ scope.row.payType }}</p>
                <div slot="reference" class="name-wrapper">
                  <el-tag size="medium">{{ scope.row.chargeType }}</el-tag>
                </div>
              </el-popover>
            </template>
          </el-table-column>
          <el-table-column label="支付金额" width="100">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.money }}</span>
            </template>
          </el-table-column>
        </el-table>
      </el-aside>
      <!-- 维修 -->
      <el-main>
        <h2>维修记录</h2>
        <el-table height:250px :data="tableData" style="width: 100%">
          <el-table-column label="维修日期" width="180">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.fixTime }}</span>
            </template>
          </el-table-column>
          <el-table-column label="维修类型" width="180">
            <template slot-scope="scope">
              <el-popover trigger="hover" placement="top">
                <p>报修学生学号: {{ scope.row.sId }}</p>
                <p>工人ID: {{ scope.row.workerId }}</p>
                <p>维修描述: {{ scope.row.description }}</p>
                <div slot="reference" class="name-wrapper">
                  <el-tag size="medium">{{ scope.row.fixType }}</el-tag>
                </div>
              </el-popover>
            </template>
          </el-table-column>
          <el-table-column label="维修状态" width="100">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.completed }}</span>
            </template>
          </el-table-column>
          <el-table-column label="操作">
            <template slot-scope="scope">
              <el-button size="mini" @click="ensure(scope.$index, scope.row)">确认维修</el-button>
            </template>
          </el-table-column>
        </el-table> <br>
        <el-row :gutter="0">
          <el-col :span="4" :offset="20">
            <div>
              <el-button type="primary" @click="dialogFormVisible = true">报修</el-button>
              <!-- 弹窗 -->
              <el-dialog title="缴费" :visible.sync="dialogFormVisible">
                <el-form :model="fixForm">
                  <el-form-item label="学号">
                    <el-input v-model="fixForm.sId"> </el-input>
                  </el-form-item>
                  <el-form-item label="寝室ID">
                    <el-input v-model="fixForm.dormId"> </el-input>
                  </el-form-item>
                  <el-form-item label="维修类型">
                    <el-input v-model="fixForm.fixType"> </el-input>
                  </el-form-item>
                  <el-form-item label="维修描述">
                    <el-input v-model="fixForm.description"> </el-input>
                  </el-form-item>
                </el-form>
                <div slot="footer" class="dialog-footer">
                  <el-button @click="dialogFormVisible = false">取 消</el-button>
                  <el-button type="primary" @click="
  dialogFormVisible = false;
onsure1();
                  ">确 定</el-button>
                </div>
              </el-dialog>
            </div>
          </el-col>
        </el-row>

      </el-main>
    </el-container>
  </div>
</template>
<script>
import { nanoid } from "nanoid";
export default {
  data() {
    return {
      dialogChargeFormVisible: false,
      dialogFormVisible: false,
      dorm: [{
        dormId: "",
        waterBalance: "1",
        eletricBalance: "1",
      }],
      chargeForm: {
        sId: "1111",
        dormId: "jjiji",
        chargeType: "sadasd",
        payType: "",
        money: "",
      },
      tableData: [
        {
          fixId: "111",
          sId: "44",
          dormId: "4554",
          fixType: "222",
          fixTime: "5454",
          description: "大师傅你垃圾啊发布会甲BKJFBSAJFB JALSBF JL ABSFJ.B AJDLAB FLJBbJJJ BJB FJB减肥包括技术大部分就是解放垃圾封闭空间吧撒阿萨我否无奇服务器服务器废弃物废弃物服务废弃物f请问fwqadasd dsfsdjfkjabgljbahgdahgowaeghwaheipgnawefnwajnfnfiawnfpiawjnefehwnafhowo奥术大师多",
          workerId: "45",
          completed: "1"
        }],
      transaction: [{
        trasactionId: "1111",
        chargeTime: "",
        dormId: "",
        money: "",
        sId: "",
        payType: "",
        chargeType: "",
      }],
      fixForm: {
        sId: "11111",
        dormId: "",
        fixType: "",
        description: ""
      }
    };
  },
  created() {
console.log(this.fixForm.sId);
  },
  methods: {


    repair() {
      console.log(this.fixForm.fixId)
    },
    onsure() {

    },
    onsure1(){

    }
  },
};
</script>
<style lang="less" scoped>
</style>