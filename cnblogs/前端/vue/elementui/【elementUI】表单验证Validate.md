## element-ui rule required动态判断

不能写在scripts中,无效，只能第一次启用，被修改后无效

```javascript
rules: {
        patientName: [
          { required: this.isApp, message: '请输入姓名', trigger: 'blur' }
        ]
}
```

需要写在form-item中
```html
<el-form-item
                label="移植类型"
                prop="planCode"
                :rules="{required:this.typeId===1,message:'请选择移植类型',trigger: 'change'}"
              >
                <el-select v-model="dto.planCode" style="width:100%" placeholder="请选择">
                  <el-option
                    v-for="item in migrationTypeList"
                    :key="item.id"
                    :label="item.transplantName"
                    :value="item.id"
                  />
                </el-select>
                <!-- <el-input v-model="form.name"></el-input> -->
              </el-form-item>
```

## vue rule 对象属性校验

```html
 <el-form :model="model" :rules="modelRules">
    <el-form-item prop="information.name" label="方案名称">
                  <el-input v-model="model.information.name" />
                </el-form-item>
                 <el-form-item prop="information.code" label="方案code">
                  <el-input v-model="model.information.code" />
                </el-form-item>
 </el-form>
```

javascript校验部分
```javascript
 model: {
        information: {
          id: 0,
          code: null,
          name: null,
          recommendedExclusionCriteria: null,
          recommendedInclusionCriteria: null,
          researchCycle: null,
          researchObjective: null
        },
        drugs: [],
        researchProcesses: [],
        researchGroups: []
      },
modelRules: {
        'information.name': [
          { required: true, message: '方案名称不能为空', trigger: 'change' }
        ],
        'information.code': [
          { required: true, message: '方案code不能为空', trigger: 'change' }
        ]
      }
```

或者rules写成

```javascript
modelRules: {
        information: {
          name: [{ required: true, message: '方案名称不能为空', trigger: 'change' }],
          code: [{ required: true, message: '方案code不能为空', trigger: 'change' }]
        }
}
```