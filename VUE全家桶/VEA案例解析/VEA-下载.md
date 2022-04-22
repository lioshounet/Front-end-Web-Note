# VEA下载

## 主要内容

excel表格生成并下载

zip下载

pdf下载

## excel表格生成并下载

### 依赖

`````bash
npm install xlsx file-saver -S
npm install script-loader -S -D
`````

vendor

Export2Excel.js

### 代码

```````js
 import("@/vendor/Export2Excel").then((excel) => {
        const tHeader = ["中文名", "感染", "治疗", "死亡"];
        const filterVal = ["name", "confirm", "heal", "dead"];
        const list = this.tableData;
        const data = this.formatJson(filterVal, list);
        console.log(data);
        excel.export_json_to_excel({
          header: tHeader,
          data,
          filename: "世界疫情数据",
          autoWidth: true,
          bookType: "xlsx",
        });
      });
```````

### 要点

因为库比较大因此动态引入

其中主要参数是

- 文件头

- json要提取的数据

- json本体

- json转化函数

  ```js
      formatJson(filterVal, jsonData) {
        return jsonData.map((v) =>
          filterVal.map((j) => {
            if (j === "timestamp") {
              return parseTime(v[j]);
            } else {
              return v[j];
            }
          })
        );
      },
  ```

### 依赖版本问题

xlsx--0.14.1最保险

## zip下载

依赖问题

```````bash
npm i jszip #3.2.1
```````

zip的js

vendor

Export2Zip

### 代码

``````js
    zipOut() {
      import("@/vendor/Export2Zip").then((zip) => {
        const tHeader = ["中文名", "感染", "治疗", "死亡"];
        const filterVal = ["name", "confirm", "heal", "dead"];
        const list = this.tableData;
        const data = this.formatJson(filterVal, list);
        console.log(data);
        zip.export_txt_to_zip(tHeader, data, "世界疫情数据", "世界疫情数据");
      });
    },
``````

## pdf导出

### 核心原理

window.print();

用这个函数把当下网页的所有样式作为pdf导出

因此文章要单独做一个页面呈现

因此顺序是这样的：文章list------纯文章页面--------函数触发导出pdf的框

文章list的实现可能会用到变量scope.row-------element

`````js
 setTimeout(() => {
        this.$nextTick(() => {
          window.print();
        });
      }, 1000);
`````

