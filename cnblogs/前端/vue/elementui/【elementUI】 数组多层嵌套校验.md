## vue for 渲染控件 数组多层嵌套校验

注意prop和rules写法

```html
<div v-for="(site,siteIndex) in tempProject.tempSampleSite" :key="'site-'+siteIndex">
  <el-row>
    <el-col :span="12">
      <el-form-item
        label-width="30%"
        label="名称"
        :prop="'tempSampleSite.'+siteIndex+'.name'"
        :rules="{ required: true, message: '取样部位不能为空', trigger: 'change' }"
      >
        <el-input v-model="site.name" :maxlength="40" />
      </el-form-item>
    </el-col>
    <el-col :span="12">
      <el-button
        v-show="siteIndex==tempProject.tempSampleSite.length-1"
        type="text"
        size="mini"
        style="margin-left: 20px;"
        @click="addSampleSite"
      >
        添加
      </el-button>
    </el-col>
  </el-row>
</div>
```

