header中有三个元素，logo，logo文字，退出按钮
logo和logo文字为一组在左侧，按钮在右侧
我们可以在header里分两组，给display:flex，justify-content: space-between样式。这样两组分别会靠左靠右。

```vue
<el-header>

  <div>
    <img src="@/assets/heima.png">
    <span>电商后台管理系统</span>
  </div>
  
  <el-button type="info" @click="logout">退出</el-button>
  
</el-header>
```

样式(less)
```
.el-header{
  background-color: #373D41;
  display: flex;
  justify-content: space-between;
  padding-left: 0px;
  align-items: center;
  color: #fff;
  font-size: 20px;
  > div{
    display: flex;
    align-items: center;

    > span{
      margin-left:15px ;
    }
  }
}
```