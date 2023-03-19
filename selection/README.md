### API
    1、list(Array)
    要遍历的数据 注意：其中title,person,username是固定数据名称不可变(想要修改可以自行到组件内部修改),其中checked为必填项且类型为布尔值
       举例： list[
                   {
                    title:'xxx',
                    checked:false,
                    person:[
                        {
                            username:'xxx'
                            checked:false
                        }
                    ]
                   },
              ]
    2、itemStyle(Object)
    子选项的样式

    事件  @click    接受两个参数 1、选中的筛选对象 2、选中的username字符串
    示范：
    <selection :list="list" :itemStyle="{paddingLeft:'0'}" @click="result"></selection>
