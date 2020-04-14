# diyTable

基于 Vue+Element 自己封装的表格(更适用于后台管理)

## 使用前配置

复制 src/diyTable.vue 里面的代码或把项目克隆下来

在需要用到的 vue 文件中 引入 diyTable 文件或在 mian.js 中引入并将其注册为全局组件

```js
// 在main.js
import diyTable from "./components/diyTable.vue"; // 引入
Vue.component("diyTable", diyTable); // 注册为全局组件
```

## 使用说明

```html
<!-- 可直接使用 -->
<diyTable
  :options="option"
  :data="listData"
  :pages="page"
  :loading="tableLoading"
  @on-load="getList"
  @selectionOperation="selectionOperation"
>
</diyTable>
<!-- 建议创一个js文件存放对table的配置 -->
```

```js
// js文件中
export const options = {
  border: true, // 表格是否需要边框
  paging: true, // 是否需要翻页及页码
  selection: true, // 是否需要多选选择框
  menu: true, // 是否需要操作栏
  align: "center", // 表格内容居中设置还可填 left、right
  column: [
    {
      label: "订单1", // 表头
      prop: "order1", // 现对于的内容
      width: "180", // 宽度
    },
    {
      label: "订单2",
      prop: "order2",
      slot: true, // 插入设置
    },
  ],
};
```

```js
data() {
    return {
      options, // 引入options
      page: {
        total: 0, // 总页数
        currentPage: 1, // 当前页数
        pageSize: 10 // 每页显示多少条
      },
      listData: [], // 列表数据
      tableLoading: true // 加载...
    };
  },
```

- 表属性

  | 参数            |       说明       |        类型 |              选值 | 默认值 |
  | --------------- | :--------------: | ----------: | ----------------: | -----: |
  | data            |    显示的数据    |       array |                 - |      - |
  | options         | 表的对应属性配置 |      object |                 - |      - |
  | height          |       高度       | 字符串/数字 |                 - |      - |
  | max-height      |     最大高度     | 字符串/数字 |                 - |      - |
  | stripe          |   是否为斑马纹   |     boolean |        true/false |
  | size            |       尺寸       |      string | medium/small/mini |      - |
  | ··············· |

  写的累死我了 说白了 跟 element-ui 一样的 没有的 你们提 我后续再加 我封装的还是比较简单的

- 表方法

  | 方法名             |      说明      | 参数 |
  | ------------------ | :------------: | ---: |
  | on-load            | 获取表格数据的 | page |
  | selectionOperation |  获取多选数据  | data |

  没错，我不想写了 因为基本跟 element-ui 一样的

重点说一下 slot 啥叫插入 基本上就是你一个表格中某一条属性 显示的需要极具风格

这时候是不是做不到满足 那么你就可以用 slot 基本使用请看！

```js
// 先在options里的column
{
  label: "订单2",
  prop: "order2",
  slot: true, // 插入设置
},
```

```html
<diyTable
  :options="option"
  :data="listData"
  :pages="page"
  :loading="tableLoading"
  @on-load="getList"
  @selectionOperation="selectionOperation"
>
  <template slot-scope="scope" slot="terminal">
    <!-- slot 填 对应的prop -->
    <p>${{scope.row.terminal}}</p>
  </template>
  <!-- 操作也需要 -->
  <template slot-scope="scope" slot="menu">
    <el-button type="text" @click="details(scope.row)">查看详情</el-button>
  </template>
</diyTable>
```

### 好了！就这样 爱咋的咋得吧
