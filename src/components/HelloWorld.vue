<template>
  <div class="main">
      <table>
        <tr>
          <td colspan="2">Công cụ tính chỉ số NH3</td>
        </tr>
        <tr>
          <td>pH</td>
          <td><input v-model.number="ph" type="number" value="" /></td>
        </tr>
        <tr>
          <td>Nhiệt độ (C)</td>
          <td><input v-model.number="temperature" type="number" value="" /></td>
        </tr>
        <tr>
          <td>độ mặn (ppt)</td>
          <td><input v-model.number="salinity" type="number" value="" /></td>
        </tr>
        <tr>
          <td>Lượng NH3/NH4+ đo được</td>
          <td><input v-model.number="ammonia" type="number" value="" /></td>
        </tr>
        <tr>
          <td>NH3 (mg/L hay ppm)</td>
          <td>{{result}}</td>
        </tr>
        <tr>
          <td colspan="2">Khuyến cáo: <span :style="{'color': this.level}">{{message}}</span></td>
        </tr>
        <tr>
          <td colspan="2">
            Đọc thêm <a href="https://www.patroutintheclassroom.org/docs/default-source/resources/how-to-calculate-your-unionized-ammonia-levels1-(2).pdf?sfvrsn=2">tại đây</a>
          </td>
        </tr>
        <tr>
          <td colspan="2">
            Công thức tham khảo tại: <a href="https://floridadep.gov/waste/district-business-support/documents/un-ionized-ammonia-calculator">Florida Department of Environmental Protection</a>
          </td>
        </tr>
      </table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      ph: 0,
      temperature: 0,
      salinity: 0,
      ammonia: 0,
      message: " ",
      level: ""
    }
  },
  computed: {
    result: function() {
      return ((17*this.ammonia)/(14*(1+Math.pow(10, (0.09018+(2729.92/(this.temperature+273.15))+((0.1552-(0.0003142*this.temperature))*((19.9273*this.salinity)/(1000-(1.2005109*this.salinity)))))-this.ph)))).toFixed(7)
    },
  },
  watch: {
    result: function(value) {
      if (value < 0.03) {
        this.message = "An Toàn"
        this.level = "green"
      } else if (value >= 0.03 && value < 0.05) {
        this.message = "Cảnh báo cần giảm NH3"
        this.level = "#bf671b"
      } else if (value >= 0.05 && value < 2) {
        this.message = "Cá đang chết dần"
        this.level = "red"
      } else if (value >= 2) {
        this.message = "Không còn gì để chết"
        this.level = "blue"
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
label {
  margin-right: 30px;
}
input {
  width: 50px;
  text-align: right;
}
.main {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
