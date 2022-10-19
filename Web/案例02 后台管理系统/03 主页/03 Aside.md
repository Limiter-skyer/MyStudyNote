左侧是一个菜单，可折叠，最深有二级菜单。
这里推荐用el-menu组件
```vue
<el-menu
  default-active="2"
  class="el-menu-vertical-demo"
  @open="handleOpen"
  @close="handleClose"
  background-color="#333744"
  text-color="#fff"
  active-text-color="#ffd04b"
>
<!-- 一级菜单 -->
  <el-submenu index="1">
    <!-- 一级菜单模板区 -->
    <template slot="title">
      <!-- 图标 -->
      <i class="el-icon-location"></i>
      <!-- 一级菜单文本 -->
      <span>一级菜单</span>
    </template>
    
    <!-- 二级菜单 -->
    <el-menu-item index="1-4-1">
      <!-- 图标 -->
      <i class="el-icon-location"></i>
      <!-- 二级菜单文本 -->
      <span>二级菜单</span>
    </el-menu-item>
  </el-submenu>
</el-menu>
```
