---
title: summary
date: 2024-01-02 14:56:45
tags: Vue
categories: 
- programming
- Vue
---

### summary-method用法

- 参数包括 columns、 data 
  - columns：每一列的基础数据组成的数组
  - data：表格数据
- 返回值：一个包含合计行的数组

```js
getSummaries(param) {
      const { columns, data } = param;
      const sums = [];
      let lcount = 0;
      columns.forEach((column, index) => {
        lcount = 0;
        if (index === 0) {
          sums[index] = '合计';
          column.width = 120;
          return;
        }
        const values = data.map(item => typeof item[column.property] == 'string' ? Number(item[column.property].replace('%', '')) : Number(item[column.property]));
        // console.log(values);
        if (column.property.includes('time')) {
          sums[index] = "";
        }
        else if (column.property.includes('rate') || column.property === 'pass' || column.property === 'eta' || column.property === 'target') {
          let sum = 0;
          values.forEach((item) => {
            sum = sum + item;
            if (item != 0) { lcount = lcount + 1 }
          });
          if (sum == 0)
            sums[index] = '0'
          else
            sums[index] = (sum / lcount).toFixed(2) + '%';
        }
        else {
          let sum = 0;
          values.forEach((item) => {
            sum = sum + item;
          });
          if (sum == 0)
            sums[index] = 0;
          else
            sums[index] = sum.toFixed(4);
        }
      });
      return sums;
    }
```

