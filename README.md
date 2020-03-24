# 想了好久还是没想到怎么把三层嵌套循环减少，所有思路转变称 map 结构的数据，应该只需要遍历一次

从开始日期一次加一天，直到等于结束日期
当年份对象没有年份时，插入，月份，日同理，最后做一次数据整合时间复杂度会从比如（2010-01-01 ～-2020-01-01）10 _ 12 _ 30 变成 12\*30 + 1

我微信：13691035516 随时骚扰哈

我完善了一下代码，转成 map 结构处理后，时间复杂度是下降了，但是效率没有提升。。。。我想是不是我用了 moment 插件引起的


## 完善了下，昨晚写的太着急了

 - 先获取所有日的情况是一个map结构，
 - 在获取所有月的情况是一个数组，0表示闰年，1表示不是
 - 在获取年的组成数据格式
 - 获取日的是n 获取月的n 获取年的也是n 复杂度 n+n+n

```this.isLeapYear = function(year) {
      let res = false;
      if (
        (year % 4 == 0 && year % 100 != 0) ||
        (year % 100 == 0 && year % 400 == 0)
      ) {
        res = true;
      }
      return res;
    };

    this.getAllDays = function() {
      const maxDays = [];
      for (let index = 1; index <= 31; index++) {
        maxDays.push({
          lable: `${index} 日`,
          value: index
        });
      }
      return {
        "28": maxDays.slice(0, 28),
        "29": maxDays.slice(0, 29),
        "30": maxDays.slice(0, 30),
        "31": maxDays.slice(0, 31)
      };
    };

    this.getAllMonths = function(daysMap) {
      let res = [];
      const length31 = [1, 3, 5, 7, 8, 10, 12];
      for (let index = 1; index <= 12; index++) {
        if (length31.includes(index)) {
          res.push({
            lable: `${index} 月`,
            value: index,
            childs: daysMap["31"]
          });
        } else if (index === 2) {
          res.push({
            lable: `${index} 月`,
            value: index,
            childs: []
          });
        } else {
          res.push({
            lable: `${index} 月`,
            value: index,
            childs: daysMap["30"]
          });
        }
      }
      const res1 = res.concat();
      const res2 = res.concat();
      res1[0].childs = daysMap["29"];
      res2[1].childs = daysMap["28"];
      return [res1, res2];
    };

    this.getDateObj4 = function(start, end) {
      const daysMap = this.getAllDays();
      const monthMap = this.getAllMonths(daysMap);
      const res = [];
      for (let index = start; index <= end; index++) {
        const isLeapYear = this.isLeapYear(index);
        res.push({
          lable: `${index} 年`,
          value: index,
          childs: isLeapYear ? monthMap[0] : monthMap[1]
        });
      }
      return res;
    };
```
