<template>
  <div class="main"  >
  <table>
      <!-- <caption>土壤墒情</caption> -->
    <tr>
    <th id="n1">检测参数</th>
    <th>当前值</th>
    <th>当前时间</th>
    </tr>
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
            const month = now.getMonth();
            const day = now.getDate();
            const hours = now.getHours();
            const minutes = now.getMinutes();
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
    /* 阴影 */
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
    /*取消表格边框 */
    border-collapse:collapse;
    /* 文字缩进 */
    letter-spacing:1px;
}
tr{ /* 表格横线 */
    border-bottom:1px solid #ffffff;
    color:#fcf7f7cc;
}

#n1 {
  text-align: left;
}
</style>