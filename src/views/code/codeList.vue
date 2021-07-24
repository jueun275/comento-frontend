<template>
  <div>
    <layout/>
    <el-radio-group v-model="ui" class="mt20">
      <el-radio-button label="html">html</el-radio-button>
      <el-radio-button label="element">element-ui</el-radio-button>
    </el-radio-group>
    <div v-if="ui == 'html'">
      <div class="divLeft">
        <input
          type="text"
          class="searchInput"
          v-model="form.commCdNm"
          placeholder="코드명"
          style="display: table-cell; width: 90%"
         />
        <input class="searchBtn" type="button" @click="searchCode" value="조회" />
      </div>
      <div
        class="mt10"
        style="height: 300px; overflow-y: scroll; overflow-x: auto"
      >
        <div style="text-align: left">
          <span>대표코드</span>
        </div>
        <div>
          <table style="width: 100%" class="mt10">
            <colgroup>
              <col width="10%" />
              <col width="20%" />
              <col width="20%" />
              <col width="10%" />
              <col width="10%" />
              <col width="10%" />
              <col width="10%" />
              <col width="10%" />
            </colgroup>
            <thead>
              <td scope="col">코드</td>
              <td scope="col">코드명</td>
              <td scope="col">코드영문명</td>
              <td scope="col">코드순서</td>
              <td scope="col">사용여부</td>
              <td scope="col">연결코드1</td>
              <td scope="col">연결코드2</td>
              <td scope="col">연결코드3</td>
            </thead>
            <tbody>
              <tr
                v-for="item in codeList"
                @click ="clickRow(item)"
                :key="item.commCdId"
                style="cursor: pointer"
              >
                <td>
                  {{ item.commCdId }}
                </td>
                <td>{{ item.commCdNm }}</td>
                <td>{{ item.commCdEng }}</td>
                <td>{{ item.cdSort }}</td>
                <td>{{ item.cdUseYn }}</td>
                <td>{{ item.conn1Cd }}</td>
                <td>{{ item.conn2Cd }}</td>
                <td>{{ item.conn3Cd }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
      <div class="mt10">
        <div style="text-align: left">
          <span>상세코드</span>
        </div>
        <table style="width: 100%" class="mt10">
          <colgroup>
            <col width="10%" />
            <col width="5%" />
            <col width="20%" />
            <col width="15%" />
            <col width="10%" />
            <col width="10%" />
            <col width="10%" />
            <col width="10%" />
            <col width="10%" />
          </colgroup>
          <thead>
            <td scope="col">대표코드</td>
            <td scope="col">코드</td>
            <td scope="col">코드명</td>
            <td scope="col">코드영문명</td>
            <td scope="col">코드순서</td>
            <td scope="col">사용여부</td>
            <td scope="col">연결코드1</td>
            <td scope="col">연결코드2</td>
            <td scope="col">연결코드3</td>
          </thead>
          <tbody>
            <tr v-for="item in codeInfoList" :key="item.cdId">
              <td>{{ item.commCdId }}</td>
              <td>{{ item.cdId }}</td>
              <td>{{ item.cdNm }}</td>
              <td>{{ item.cdEng }}</td>
              <td>{{ item.cdSort }}</td>
              <td>{{ item.cdUseYn }}</td>
              <td>{{ item.conn1Cd }}</td>
              <td>{{ item.conn2Cd }}</td>
              <td>{{ item.conn3Cd }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div v-if="ui == 'element'" class="mt10">
      <div class="divLeft">
        <el-input
          v-model="form.commCdNm"
          placeholder="코드명"
          style="width: 90%"
        ></el-input>
        <el-button @click="searchCode" type="primary">조회</el-button>
      </div>
      <el-card class="box-card mt10">
        <div slot="header" class="clearfix" style="text-align: left">
          <span>대표코드</span>
        </div>
        <el-row type="flex">
          <el-table
            :data="codeList"
            border
            highlight-current-row
            @row-click = "clickRow"
            height="400"
          >
            <el-table-column align="center" label="코드" width="200">
              <template slot-scope="scope">
                {{ scope.row.commCdId }}
              </template>
            </el-table-column>
            <el-table-column align="center" label="코드명" width="200">
              <template slot-scope="scope">
                {{ scope.row.commCdNm }}
              </template>
            </el-table-column>
            <el-table-column label="코드영문명" align="center" width="200">
              <template slot-scope="scope">
                {{ scope.row.commCdEng }}
              </template>
            </el-table-column>
            <el-table-column label="코드순서" align="center" width="200">
              <template slot-scope="scope">
                {{ scope.row.cdSort }}
              </template>
            </el-table-column>
            <el-table-column label="사용여부" align="center" width="150">
              <template slot-scope="scope">
                {{ scope.row.cdUseYn }}
              </template>
            </el-table-column>
            <el-table-column label="연결코드1" align="center" width="150">
              <template slot-scope="scope">
                {{ scope.row.conn1Cd }}
              </template>
            </el-table-column>
            <el-table-column label="연결코드2" align="center" width="150">
              <template slot-scope="scope">
                {{ scope.row.conn2Cd }}
              </template>
            </el-table-column>
            <el-table-column label="연결코드3" align="center" width="150">
              <template slot-scope="scope">
                {{ scope.row.conn3Cd }}
              </template>
            </el-table-column>
          </el-table>
        </el-row>
      </el-card>

      <el-card class="box-card mt10">
        <div slot="header" class="clearfix" style="text-align: left">
          <span>상세코드</span>
        </div>
        <el-row type="flex">
          <el-table
            :data="codeInfoList"
            border
            highlight-current-row
            height="300"
          >
            <el-table-column align="center" label="대표코드" width="200">
              <template slot-scope="scope">
                {{ scope.row.commCdId }}
              </template>
            </el-table-column>
            <el-table-column align="center" label="코드" width="200">
              <template slot-scope="scope">
                {{ scope.row.cdId }}
              </template>
            </el-table-column>
            <el-table-column label="코드명" align="center" width="200">
              <template slot-scope="scope">
                {{ scope.row.cdNm }}
              </template>
            </el-table-column>
            <el-table-column label="코드영문명" align="center" width="200">
              <template slot-scope="scope">
                {{ scope.row.cdEng }}
              </template>
            </el-table-column>
            <el-table-column label="코드순서" align="center" width="150">
              <template slot-scope="scope">
                {{ scope.row.cdSort }}
              </template>
            </el-table-column>
            <el-table-column label="사용여부" align="center" width="150">
              <template slot-scope="scope">
                {{ scope.row.cdUseYn }}
              </template>
            </el-table-column>
            <el-table-column label="연결코드1" align="center" width="120">
              <template slot-scope="scope">
                {{ scope.row.conn1Cd }}
              </template>
            </el-table-column>
            <el-table-column label="연결코드2" align="center" width="120">
              <template slot-scope="scope">
                {{ scope.row.conn2Cd }}
              </template>
            </el-table-column>
            <el-table-column label="연결코드3" align="center" width="120">
              <template slot-scope="scope">
                {{ scope.row.conn3Cd }}
              </template>
            </el-table-column>
          </el-table>
        </el-row>
      </el-card>
    </div>
  </div>
</template>

<script>
import layout from '@/views/layout/Layout'
import { reqPost } from '@/api/tran'

export default {
  name: 'CodeList',
  components: { layout },
  data () {
    return {
      form: {
        commCdNm: ''
      },
      codeInfoList: [],
      codeList: [],
      ui: 'html'
    }
  },
  methods: {
    searchCode () {
      console.log(this.form.commCdNm)
      reqPost('/code/findCodeByNm', this.form).then((response) => {
        console.log(response)
        this.codeList = response.data.list
      })
    },
    clickRow (row) {
      console.log(row)
      reqPost('/code/selectCodeList/' + row.commCdId).then((response) => {
        if (response.data.list.length === 0) {
          alert('결과가 없습니다')
        }
        this.codeInfoList = response.data.list
      })
    }
  }
}
</script>
