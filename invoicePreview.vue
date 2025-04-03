<template>
  <div class="invoice-preview">
    <div class="header">
      <span class="invoice-title" style="font-size: xx-large;">
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;电子发票（增值税专用发票）
      </span>
      <span style="padding-left: 20px;field-sizing: fixed;">发票号码：{{ invoiceNumber }}</span><br />
      <span style="padding-left: 564px;">开票日期：{{ invoiceDate }}</span>
    </div>
    <div class="main-content">
      <div class="buyer-seller-section">
        <p class="vertical-text">购买方信息</p>
        <div class="vertical-line"></div>
        <div class="buyer-info">
          <span class="text-style">名 称：</span>{{ gmfmc }}<br /><br />
          <span class="text-style">统一社会信用代码/纳税人识别号：</span>{{ gmfnsrsbh }}
        </div>
        <div class="vertical-line"></div>
        <p class="vertical-text">销售方信息</p>
        <div class="vertical-line"></div>
        <div class="seller-info">
          <span class="text-style">名 称：</span>{{ xsfmc }}<br /><br />
          <span class="text-style">统一社会信用代码/纳税人识别号：</span>{{ xsfnsrsbh }}
        </div>
      </div>
      <table class="invoice-table">
        <thead>
          <tr class="text-style" style="text-align: right;">
            <th>项目名称</th>
            <th>规格型号</th>
            <th>单 位</th>
            <th>数 量</th>
            <th>单 价</th>
            <th>金 额</th>
            <th>税率/征收率</th>
            <th>税 额</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in items" :key="item.id">
            <td>{{ item.xmmc }}</td>
            <td>{{ item.ggxh }}</td>
            <td>{{ item.dw }}</td>
            <td>{{ item.sl }}</td>
            <td>{{ item.dj }}</td>
            <td>{{ item.je }}</td>
            <td>{{ item.slv * 100 }}%</td>
            <td>{{ item.se }}</td>
          </tr>
        </tbody>
        <br /><br /><br /><br /><br /><br />
        <tfoot>
          <tr>
            <td colspan="2" class="text-style">合&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;计</td>
            <td colspan="4" style="text-align: right;">￥{{ hjje }}</td>
            <td colspan="4" style="text-align: right;">￥{{ hjse }}</td>
          </tr>
        </tfoot>
      </table>
      <div class="total-price-section">
        <div class="total-price-left">
          <span class="text-style" style="width: 22%;text-align: center;">价税合计（大写）</span>
        </div>
        <div class="vertical-line"></div>
        <div class="total-price-middle" style="padding-left: 20px;">
          <span>⨂ {{ jshjUpper }}</span>
        </div>
        <div class="total-price-right">
          <span class="text-style">（小写）</span>¥{{ formatCurrency(jshj) }}
        </div>
      </div>
      <div class="remark-section">
        <div class="bank-info">
          <p class="vertical-text">备注</p>
          <div class="vertical-line"></div>
          <div class="bank-column">
            <span class="text-style">购方开户银行：</span>{{ gmfkhh }}<br />
            <span class="text-style">销方开户银行：</span>{{ skyhmc }}<br />
            <span>{{ bz }}</span>
          </div>
          <div class="bank-number">
            <span class="text-style">银行账号: </span>{{ gmfzh }}<br />
            <span class="text-style">银行账号: </span>{{ skyhzh }}
          </div>
        </div>
      </div>
      <div class="maker">
        <span class="text-style">开票人：</span>{{ maker }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'invoicePreviewModal',
  data() {
    return {
      visible: false,
      width: '80%',
      invoiceNumber: '',
      invoiceDate: '',
      gmfmc: '',
      gmfnsrsbh: '',
      xsfmc: '',
      xsfnsrsbh: '',
      items: [],
      hjje: '',
      hjse: '',
      jshjUpper: '',
      jshj: '',
      remarkText: '',
      maker: '',
      gmfkhh: '',
      gmfzh: '',
      skyhmc: '',
      skyhzh:'',
      bz: '',
    }
  },
  methods: {
    formatCurrency(value) {
      if (isNaN(value)) return '0.00';
      return Number(value).toFixed(2)
        .replace(/\d(?=(\d{3})+\.)/g, '$&,');
    },
    convertToChinese(num) {
    const units = ['', '拾', '佰', '仟', '万', '拾万', '佰万', '仟万', '亿', '拾亿', '佰亿', '仟亿', '兆']
    const digits = ['零', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖']
    const numStr = num.toFixed(2).toString()
    const [integerPart, decimalPart] = numStr.split('.')
    let chineseStr = ''
    for (let i = 0; i < integerPart.length; i++) {
      const digit = integerPart[integerPart.length - 1 - i]
      chineseStr = `${digits[digit]}${units[i]}${chineseStr}`
    }
    chineseStr += '圆'
    if (decimalPart) {
      if (decimalPart[0] !== '0') {
        chineseStr += `${digits[decimalPart[0]]}角`
      }
      if (decimalPart[1] !== '0') {
        chineseStr += `${digits[decimalPart[1]]}分`
      }
    } else {
      chineseStr += '整'
    }
    return chineseStr
  },
  }
}
</script>

<style scoped>
.invoice-preview {
  width: 900px;
  margin: 0 auto;
  padding: 15px;
}
.header {
  text-align: center;
  color: #7f0000;
  font-family: cursive;
}
.header h2 {
  color: #7f0000;
  text-decoration: underline;
  padding-bottom: 5px;
}
.main-content {
  margin-top: 15px;
}
.buyer-seller-section {
  display: flex;
  border: 2px solid #7f0000; /* 购买方销售方外框 */
}
.buyer-info, .seller-info {
  width: 50%;
  padding-top: 20px;
}
.vertical-line {
  width: 2px;
  background-color: #7f0000; /* 中间竖线 */
}
.invoice-table {
  width: 100%;
  border-collapse: collapse;
  border-left: 2px solid #7f0000;
  border-right: 2px solid #7f0000;
  border-bottom: 2px solid #7f0000;
}
.invoice-table th, .invoice-table td {
  text-align: center;
  border: none; /* 移除单元格的边框 */
}
.total-price-section {
  display: flex;
  border-left: 2px solid #7f0000; /* 价税合计上边框 */
  border-right: 2px solid #7f0000; /* 价税合计下边框 */
}
.total-price-left, .total-price-middle, .total-price-right {
  padding: 5px;
}
.total-price-middle {
  flex-grow: 1;
  text-align: left;
}
.remark-section {
border: 2px solid #7f0000;
min-height: 60px;
}

.bank-info {
  display: flex;
  justify-content: space-between;
}

.bank-column {
  width: 50%;
  padding: 10px 0px;
}
.bank-number{
  padding: 10px 0 0 2px;
  width: 47%;
}
.maker {
  text-align: left;
  margin-top: 10px;
}

.vertical-text {
  writing-mode: vertical-rl;
  letter-spacing: 2px;
  padding: 15px 0;
  text-align: center; 
  margin: 0; /* 移除外边距 */
  display: inline-block; /* 确保容器只占用所需的空间 */
  color: #7f0000;
  font-family: cursive;
}
.text-style {
  color: #7f0000;
  font-family: cursive;
}
.invoice-title {
    text-align: center;
    font-size: 24px;
    margin-bottom: 20px;
    position: relative;
    padding-bottom: 10px; /* 预留空间显示下划线 */
}

.invoice-title:before,
.invoice-title:after {
    content: '';
    position: absolute;
    left: 57%;
    transform: translateX(-50%);
    width: 61%;
    height: 2px;
    background-color: #7f0000;
}

.invoice-title:before {
    bottom: 0; /* 第一条下划线位置 */
}

.invoice-title:after {
    bottom: 5px; /* 第二条下划线与第一条的间距 */
}
</style>
