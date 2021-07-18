<template>
  <div>
    <layout/>
    <div class="divLeft">
      <input type="text" class="searchInput" v-model="form.codeNm" placeholder="코드명" style="display:table-cell;width:90%"/>
      <input class="searchBtn" type="button" @click="getList" value="조회"/>
    </div>
    <div class="mt10" style="height:300px">
      <div style="text-align:left">
        <span>대표코드</span>
      </div>
      <div>
      <table style="width:100%" class="mt10">
        <colgroup>
          <col width="10%">
          <col width="20%">
          <col width="20%">
          <col width="10%">
          <col width="10%">
          <col width="10%">
          <col width="10%">
          <col width="10%">
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
          <tr v-for="item in codeInfoList" :key="item.commCdId" @click:row="clickRow">
            <td @click="clickRow(item.commCdId)">{{item.commCdId}}</td>
            <td>{{item.commCdNm}}</td>
            <td>{{item.commCdEng}}</td>
            <td>{{item.cdSort}}</td>
            <td>{{item.cdUseYn}}</td>
            <td>{{item.conn1Cd}}</td>
            <td>{{item.conn2Cd}}</td>
            <td>{{item.conn3Cd}}</td>
          </tr>
        </tbody>
      </table>
      </div>
    </div>
    <div class="mt10">
      <div style="text-align:left">
        <span>상세코드</span>
      </div>
      <table style="width:100%" class="mt10">
        <colgroup>
          <col width="10%">
          <col width="5%">
          <col width="20%">
          <col width="15%">
          <col width="10%">
          <col width="10%">
          <col width="10%">
          <col width="10%">
          <col width="10%">
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
          <tr v-for="item in codeList" :key="item.cdId">
            <td>{{item.commCdId}}</td>
            <td>{{item.cdId}}</td>
            <td>{{item.cdNm}}</td>
            <td>{{item.cdEng}}</td>
            <td>{{item.cdSort}}</td>
            <td>{{item.cdUseYn}}</td>
            <td>{{item.conn1Cd}}</td>
            <td>{{item.conn2Cd}}</td>
            <td>{{item.conn3Cd}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import layout from '@/views/layout/Layout'
import {reqPost} from '@/api/tran'
import axios from 'axios'

export default {
  name: 'CodeList',
  components: { layout },
  data () {
    return {
      form: {
        codeNm: ''
      },
      codeInfoList: [],
      codeList: []
    }
  },
  methods: {
    getList () {
      axios.get('http://localhost:8888/code/selectCodeInfo').then(response => {
        console.log(response)
        this.codeInfoList = response.data.list
      })
    },
    doList () {
      reqPost('/code/selectCodeInfo', this.form).then(response => {
        this.codeInfoList = response.data.list
      })
    },
    clickRow (userCd) {
      axios.post('http://localhost:8888/code/selectCodeList/' + userCd).then(response => {
        console.log(userCd + '컬럼을 눌렀습니다')
        console.log(response)
        this.codeList = response.data.list
      })
    }
  }
}
</script>
