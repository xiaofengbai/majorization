# 想了好久还是没想到怎么把三层嵌套循环减少，所有思路转变称 map 结构的数据，应该只需要遍历一次

从开始日期一次加一天，直到等于结束日期
当年份对象没有年份时，插入，月份，日同理，最后做一次数据整合时间复杂度会从比如（2010-01-01 ～-2020-01-01）10 _ 12 _ 30 变成 12\*30 + 1

我完善了一下代码，转成map结构处理后，时间复杂度是下降了，但是效率没有提升。。。。我想是不是我用了moment插件引起的

````this.getDateObj2 = function(start, end) {
    let res = [];
    let yearsObj = {};
    let curDate = start;
    while (curDate.isBefore(end)) {
      curDate = curDate.add("day", 1);
      let cutYears = curDate.year();
      let cutMonth = curDate.month();
      let cutDay = curDate.date();
      if (!yearsObj[cutYears]) {
        yearsObj[cutYears] = {
          lable: "year",
          value: cutYears,
          childs: {
            [cutMonth + 1]: {
              lable: "month",
              value: cutMonth + 1,
              childs: {
                [cutDay]: { lable: "day", value: cutDay, childs: null }
              }
            }
          }
        };
      } else {
        if (!yearsObj[cutYears].childs[cutMonth + 1]) {
          yearsObj[cutYears].childs[cutMonth + 1] = {
            lable: "month",
            value: cutMonth + 1,
            childs: {}
          };
        }
        if (!yearsObj[cutYears].childs[cutMonth + 1].childs[cutDay]) {
          yearsObj[cutYears].childs[cutMonth + 1].childs[cutDay] = {
            lable: "day",
            value: cutDay,
            childs: null
          };
        }
      }
    }
    return yearsObj;
  };```
````
