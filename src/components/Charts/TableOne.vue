<template>
  <div class="main"  >
  <table>
      <!-- 标题> -->
    <tr>
    <th id="n1">检测参数</th>
    <th>当前值</th>
    <th>更新时间</th>
    </tr>
    <!-- 列表参数 -->
    <tr>
        <td>土壤温度</td>
        <td>{{Currentdata2.temperature}}℃</td>
        <td>{{ Time }}</td>
    </tr>
    <tr>
        <td>土壤湿度</td>
        <td>{{Currentdata2.humidity}}%RH</td>
        <td>{{ Time }}</td>
    </tr>
    <tr>
        <td>土壤PH</td>
        <td>{{Currentdata2.PHValue}}PH</td>
        <td>{{ Time }}</td>
    </tr>
    <tr>
        <td>土壤氮</td>
        <td>{{Currentdata2.N}}mg/kg</td>
        <td>{{ Time }}</td>
    </tr>
    <tr>
        <td>土壤磷</td>
        <td>{{Currentdata2.P}}mg/kg</td>
        <td>{{ Time }}</td>
    </tr>
    <tr>
        <td>土壤钾</td>
        <td>{{Currentdata2.K}}mg/kg</td>
        <td>{{ Time }}</td>
    </tr>
    <tr>
        <td>土壤电导率</td>
        <td>{{Currentdata2.conduct}}us/cm</td>
        <td>{{ Time }}</td>
    </tr>
</table> 
</div>
</template>

<script>
export default{
    props:["Currentdata2"],
    data() {
        return {
            Time: ""
        };
    },
    methods: {
        getCurrentTime() {
            const now = new Date();
            const year = now.getFullYear();
            const month = String(now.getMonth()+1).padStart(2,'0');
            const day = String(now.getDate()).padStart(2,'0');
            const hours = String(now.getHours()).padStart(2,'0');
            const minutes = String(now.getMinutes()).padStart(2,'0');
            this.Time = `${year}-${month}-${day} ${hours}:${minutes}`;
        }
    },
    created() {
        this.getCurrentTime();
    },
}

</script>
<style>
table{
    /*取消表格边框 */
    border-collapse:collapse;
    /* 文字缩进 */
    letter-spacing:1px;
    /* background-color: transparent ; */
    width: 100%;
}
tr{
    color:#fcf7f7cc;
    opacity: 0.7;
}
th,td{
    font-size:  0.9em;
}
#n1 {
  text-align: left;
}
</style>