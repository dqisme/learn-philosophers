<template>
  <div ref="visualization">
  </div>
</template>

<script>
import vis from 'vis'
import 'vis/dist/vis.min.css'

const date = function (year, month = 1, date = 1) {
  return new Date(year, month - 1, date)
}

export default {
  name: 'Home',
  mounted () {
    const philosophies = [
      {content: '泰勒斯', start: date(-624), end: date(-546), group: '古希腊'},
      {content: '阿那克西曼德', start: date(-610), end: date(-545), group: '古希腊'},
      {content: '阿那克西美尼', start: date(-570), end: date(-526), group: '古希腊'},
      {content: '赫拉克利特', start: date(-540), end: date(-480), group: '古希腊'},
      {content: '巴门尼德', start: date(-515), end: date(-445), group: '古希腊'},
      {content: '苏格拉底', start: date(-470), end: date(-399), group: '古希腊'},
      {content: '德谟克里特', start: date(-460), end: date(-370), group: '古希腊'},
      {content: '柏拉图', start: date(-428), end: date(-348), group: '古希腊'},
      {content: '亚里士多德', start: date(-384), end: date(-322, 3, 7), group: '古希腊'},
      {content: '奥古斯丁', start: date(354, 11, 13), end: date(430, 8, 28), group: '罗马帝国'},
      {content: '托马斯·阿奎那', start: date(1225), end: date(1274, 3, 7), group: '西西里王国'},
      {content: '弗朗西斯·培根', start: date(1561, 1, 22), end: date(1626, 4, 9), group: '英国'},
      {content: '伽利略·伽利莱', start: date(1564, 2, 15), end: date(1642, 1, 8), group: '意大利'},
      {content: '托马斯·霍布斯', start: date(1588, 4, 5), end: date(1679, 12, 4), group: '英国'},
      {content: '勒内·笛卡尔', start: date(1596, 3, 31), end: date(1650, 2, 11), group: '法国'},
      {content: '布莱兹·帕斯卡', start: date(1623, 6, 19), end: date(1662, 8, 19), group: '法国'},
      {content: '巴鲁赫·斯宾诺莎', start: date(1632, 11, 24), end: date(1677, 2, 11), group: '荷兰'},
      {content: '约翰·洛克', start: date(1632, 8, 29), end: date(1704, 10, 28), group: '英国'},
      {content: '艾萨克·牛顿', start: date(1643, 1, 4), end: date(1727, 3, 31), group: '英国'},
      {content: '戈特弗里德·莱布尼茨', start: date(1646, 7, 1), end: date(1716, 11, 14), group: '德国'},
      {content: '乔治·贝克莱', start: date(1685, 3, 12), end: date(1753, 1, 14), group: '爱尔兰'},
      {content: '孟德斯鸠', start: date(1689, 1, 18), end: date(1755, 2, 10), group: '法国'},
      {content: '伏尔泰', start: date(1694, 11, 21), end: date(1778, 5, 30), group: '法国'},
      {content: '大卫·休谟', start: date(1711, 5, 7), end: date(1776, 8, 25), group: '英国'},
      {content: '让-雅克·卢梭', start: date(1712, 6, 28), end: date(1778, 7, 2), group: '法国'},
      {content: '亚当·斯密', start: date(1723, 6, 5), end: date(1790, 7, 17), group: '英国'},
      {content: '伊曼努尔·康德', start: date(1724, 4, 22), end: date(1804, 2, 12), group: '德国'},
      {content: '杰里米·边沁', start: date(1748, 2, 15), end: date(1832, 6, 6), group: '英国'},
      {content: '黑格尔', start: date(1770, 8, 27), end: date(1831, 11, 14), group: '德国'},
      {content: '阿图尔·叔本华', start: date(1788, 2, 22), end: date(1860, 9, 21), group: '德国'},
      {content: '索伦·奥贝·克尔凯郭尔', start: date(1813, 5, 5), end: date(1855, 11, 11), group: '丹麦'},
      {content: '弗里德里希·尼采', start: date(1844, 10, 15), end: date(1900, 8, 25), group: '德国'},
      {content: '戈特洛布·弗雷格', start: date(1848, 11, 8), end: date(1925, 7, 26), group: '德国'},
      {content: '西格蒙德·弗洛伊德', start: date(1856, 5, 6), end: date(1939, 9, 23), group: '奥地利'},
      {content: '埃德蒙德·胡塞尔', start: date(1859, 4, 8), end: date(1938, 4, 26), group: '奥地利'},
      {content: '阿尔弗雷德·诺斯·怀特海', start: date(1861, 2, 15), end: date(1947, 12, 30), group: '英国'},
      {content: '伯特兰·罗素', start: date(1872, 5, 18), end: date(1970, 2, 2), group: '英国'},
      {content: '路德维希·维特根斯坦', start: date(1889, 4, 26), end: date(1951, 4, 29), group: '奥地利'},
      {content: '马丁·海德格尔', start: date(1889, 9, 26), end: date(1976, 5, 26), group: '德国'},
      {content: '鲁道夫·卡尔纳普', start: date(1891, 5, 18), end: date(1970, 9, 14), group: '德国'},
      {content: '卡尔·波普尔', start: date(1902, 7, 28), end: date(1994, 9, 17), group: '奥地利'},
      {content: '让-保罗·萨特', start: date(1905, 6, 21), end: date(1980, 4, 15), group: '法国'},
      {content: '奎因', start: date(1908, 6, 25), end: date(2000, 12, 25), group: '美国'},
      {content: '阿尔贝·加缪', start: date(1913, 11, 7), end: date(1960, 1, 4), group: '法国'},
      {content: '米歇尔·福柯', start: date(1926, 10, 15), end: date(1984, 6, 25), group: '法国'},
      {content: '雅克·德里达', start: date(1930, 7, 15), end: date(2004, 10, 9), group: '法国'}
    ]

    // philosophies.forEach((philosophy, index) => philosophy.align = 'left');

    // DOM element where the Timeline will be attached
    const container = this.$refs.visualization

    // Create a DataSet (allows two way data-binding)
    const items = new vis.DataSet(philosophies)

    // Configuration for the Timeline
    const options = {
      order: (one, another) => (one.start > another.start),
      verticalScroll: true
    }

    const groups =
      philosophies
        .reduce((result, philosophy) => result.concat(result.indexOf(philosophy.group) === -1 ? philosophy.group : []), [])
        .map((group, index) => ({id: group, content: group, order: index}))

    // Create a Timeline
    const timeline = new vis.Timeline(container, items, groups, options)

    console.log(timeline)
  }
}
</script>
