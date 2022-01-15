#### 1. el-checkbox

```js
：indeterminate="item.isIndeterminate"
```

当值为false时，呈不确定状

```vue
v-model=“item.checkAll”
```

#### 2. handleCheckAllChange(val)

```js
   //父checkBox状态
    handleCheckAllChange(val) {
      debugger
      this.checkList.forEach(item => {
        item.ishover = false
      })
      val.ishover = true
      if (!val.checkAll) {
        val.checkAll = false
        val.isIndeterminate = false
        val.checkedList = []
        if(val.title == '区域列表') {
          this.$store.state.isAreaListDisplay = false;
          this.$store.state.isAreaListLegendDisplay = false;
        }
        
        if(val.title == '监控') {
          this.$store.state.videoShow = false;
					this.$store.state.videoAndContactMutualDisplay = true;
				}
      } else {
        val.checkAll = true
        val.isIndeterminate = false
        val.checkedList = []
        val.children.forEach(item => {
          val.checkedList.push(item)
        })
        if(val.title == '区域列表') {
          this.$store.state.isAreaListDisplay = true;
          this.$store.state.isAreaListLegendDisplay = true;
        }
        if(val.title == '监控') {
          this.$store.state.videoShow = true;
					this.$store.state.videoAndContactMutualDisplay = false;
				}
      }
      
      // 所有checkboxGroup的事件
      switch (val.id) {
        case 'xfsbss':
          this.changeAllXFSBSS(val.children, !(val.checkedList.length === 0));
          break;
        case 'cqqk':
          // 判断一下上图类型[“gc”，“cqbj”，“qtqy”]
          // 是消防设施设备 TODO => changeAllXFSBSS
          // const findList = ['gc', 'cqbj', 'qtqy']
          // if(findList.find(val.value)){
          //   this.changeAllAreaVis(val.children, !(val.checkedList.length === 0));
          // }else{
          //   this.changeAllXFSBSS(val.children, !(val.checkedList.length === 0));
          // }
          break;
  
        // 否 TODO => changeAllAreaVis
        case 'qylb':
        	// debugger
          this.changeAllAreaVis(val.children, !(val.checkedList.length === 0));
          break;
      }
    },
```



#### 3. 