### elementUI dialog设置高度

dialog只有设置width的属性，无法直接设置高度
需要添加自定义class,但是style由于添加了scope无法添加对应的样式
需要添加deep

```html
    <el-dialog
      v-if="showSampleInfoEdit"
      title="样本详情编辑"
      :visible.sync="showSampleInfoEdit"
      :close-on-click-modal="false"
      width="720px"
      custom-class="patientInformation"
    >
      <PatientInformation ref="patientInformation" />
      <span slot="footer" class="dialog-footer" style="padding-right: 20px;">
        <el-button @click="showSampleInfoEdit = false">取 消</el-button>
        <el-button :loading="sampleInfoEditLoading" type="primary" @click="saveSampleInfo">保 存</el-button>
      </span>
    </el-dialog>

```


```css
<style>
  /deep/ .patientInformation {
    height: 90vh;
    overflow: auto;
  }
</style>
```


#### deep使用
deep使用编译器会报错，无法启动项目
原因：因为使用了less或scss的原因
解决方法：使用::v-/deep/或者>>> 来替换/deep/
```css
<style>
  ::v-deep .el-icon-star-on{
    color: #7cb8f0;
  }
</style>

```