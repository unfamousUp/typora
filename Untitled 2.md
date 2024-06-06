```vue
            getStudentInfo() {
                this.$axios.post(this.myHttp+'/systemCore/webView/consum?pageNum=' +
                    this.queryParams.pageNum + "&pageSize=" + this.queryParams.pageSize, this.studentInfo).then((
                    res) => {
                    this.tableData = res.data.rows
                    this.total = res.data.total
                })
            },
```

