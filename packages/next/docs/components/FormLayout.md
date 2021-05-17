# FormLayout

> 区块级布局批量控制组件，借助该组件，我们可以轻松的控制被 FormLayout 圈住的所有 FormItem 组件的布局模式

## Markup Schema 案例

```tsx
import React from 'react'
import { Input, Select, FormItem, FormLayout } from '@formily/next'
import { createForm } from '@formily/core'
import { FormProvider, createSchemaField } from '@formily/react'

const SchemaField = createSchemaField({
  components: {
    Input,
    Select,
    FormItem,
    FormLayout,
  },
})

const form = createForm()

export default () => (
  <FormProvider form={form}>
    <SchemaField>
      <SchemaField.Void
        x-component="FormLayout"
        x-component-props={{
          labelCol: 6,
          wrapperCol: 10,
        }}
      >
        <SchemaField.String
          name="input"
          title="输入框"
          x-decorator="FormItem"
          x-component="Input"
          required
        />
        <SchemaField.String
          name="select"
          title="选择框"
          x-decorator="FormItem"
          x-component="Select"
          required
        />
      </SchemaField.Void>
    </SchemaField>
  </FormProvider>
)
```

## JSON Schema 案例

```tsx
import React from 'react'
import { Input, Select, FormItem, FormLayout } from '@formily/next'
import { createForm } from '@formily/core'
import { FormProvider, createSchemaField } from '@formily/react'

const SchemaField = createSchemaField({
  components: {
    Input,
    Select,
    FormItem,
    FormLayout,
  },
})

const schema = {
  type: 'object',
  properties: {
    layout: {
      type: 'void',
      'x-component': 'FormLayout',
      'x-component-props': {
        labelCol: 6,
        wrapperCol: 10,
      },
      properties: {
        input: {
          type: 'string',
          title: '输入框',
          required: true,
          'x-decorator': 'FormItem',
          'x-component': 'Input',
        },
        select: {
          type: 'string',
          title: '选择框',
          required: true,
          'x-decorator': 'FormItem',
          'x-component': 'Select',
        },
      },
    },
  },
}

const form = createForm()

export default () => (
  <FormProvider form={form}>
    <SchemaField schema={schema} />
  </FormProvider>
)
```

## 纯 JSX 案例

```tsx
import React from 'react'
import {
  Input,
  Select,
  FormItem,
  FormButtonGroup,
  Submit,
  FormLayout,
} from '@formily/next'
import { createForm } from '@formily/core'
import { FormProvider, Field } from '@formily/react'

const form = createForm()

export default () => (
  <FormProvider form={form}>
    <FormLayout labelCol={6} wrapperCol={10}>
      <Field
        name="input"
        required
        title="输入框"
        decorator={[FormItem]}
        component={[Input]}
      />
      <Field
        name="select"
        required
        title="选择框"
        decorator={[FormItem]}
        component={[Select]}
      />
      <FormButtonGroup.FormItem>
        <Submit onSubmit={console.log}>提交</Submit>
      </FormButtonGroup.FormItem>
    </FormLayout>
  </FormProvider>
)
```

## 使用 shallow 控制 layout 上下文的传递行为

```tsx
import React from 'react'
import {
  Input,
  Select,
  FormItem,
  FormButtonGroup,
  Submit,
  FormLayout,
  IFormLayoutProps,
} from '@formily/next'
import { createForm } from '@formily/core'
import { FormProvider, Field } from '@formily/react'

const form = createForm()

const globalLayout: IFormLayoutProps = {
  inset: true,
  shallow: false,
  labelWidth: 80,
}

const sonLayout: IFormLayoutProps = {
  inset: false,
  shallow: false,
  fullness: true,
  layout: 'vertical',
}

const grandsonLayout: IFormLayoutProps = {
  layout: 'inline',
}

const LayoutContainer: React.FC<{
  layout: IFormLayoutProps
  borderColor: string
}> = (props) => {
  return (
    <div style={{ padding: '12px', border: `2px solid ${props.borderColor}` }}>
      <pre>布局配置：{JSON.stringify(props.layout, undefined, 2)}</pre>
      {props.children}
    </div>
  )
}

export default () => (
  <FormProvider form={form}>
    <LayoutContainer borderColor="#f5222d" layout={globalLayout}>
      <FormLayout {...globalLayout}>
        <Field
          name="input"
          required
          title="输入框"
          decorator={[FormItem]}
          component={[Input]}
        />
        <Field
          name="select"
          required
          title="选择框"
          decorator={[FormItem]}
          component={[Select]}
        />
        <LayoutContainer borderColor="#1890ff" layout={sonLayout}>
          <FormLayout {...sonLayout}>
            <Field
              name="input"
              required
              title="输入框"
              decorator={[FormItem]}
              component={[Input]}
            />
            <Field
              name="select"
              required
              title="选择框"
              decorator={[FormItem]}
              component={[Select]}
            />
            <LayoutContainer borderColor="#52c41a" layout={grandsonLayout}>
              <FormLayout {...grandsonLayout}>
                <Field
                  name="input"
                  required
                  title="输入框"
                  decorator={[FormItem]}
                  component={[Input]}
                />
                <Field
                  name="select"
                  required
                  title="选择框"
                  decorator={[FormItem]}
                  component={[Select]}
                />
              </FormLayout>
            </LayoutContainer>
            <FormButtonGroup.FormItem>
              <Submit>提交</Submit>
            </FormButtonGroup.FormItem>
          </FormLayout>
        </LayoutContainer>
      </FormLayout>
    </LayoutContainer>
  </FormProvider>
)
```

## hook

### useFormLayout

```ts
const layout = useFormLayout()
```

## API

| 属性名         | 类型                                        | 描述                    | 默认值     |
| -------------- | ------------------------------------------- | ----------------------- | ---------- |
| style          | CSSProperties                               | 样式                    | -          |
| className      | string                                      | 类名                    | -          |
| colon          | boolean                                     | 是否有冒号              | true       |
| labelAlign     | `'right' \| 'left'`                         | 标签内容对齐            | -          |
| wrapperAlign   | `'right' \| 'left'`                         | 组件容器内容对齐        | -          |
| labelWrap      | boolean                                     | 标签内容换行            | false      |
| labelWidth     | number                                      | 标签宽度(px)            | -          |
| wrapperWidth   | number                                      | 组件容器宽度(px)        | -          |
| wrapperWrap    | boolean                                     | 组件容器换行            | false      |
| labelCol       | number                                      | 标签宽度(24 column)     | -          |
| wrapperCol     | number                                      | 组件容器宽度(24 column) | -          |
| fullness       | boolean                                     | 组件容器宽度 100%       | false      |
| size           | `'small' \| 'default' \| 'large'`           | 组件尺寸                | default    |
| layout         | `'vertical' \| 'horizontal' \| 'inline'`    | 布局模式                | horizontal |
| direction      | `'rtl' \| 'ltr'`                            | 方向(暂不支持)          | ltr        |
| inset          | boolean                                     | 内联布局                | false      |
| shallow        | boolean                                     | 上下文浅层传递          | true       |
| feedbackLayout | `'loose' \| 'terse' \| 'popover' \| 'none'` | 反馈布局                | true       |
| tooltipLayout  | `"icon" \| "text"`                          | 问提示布局              | `"icon"`   |
| bordered       | boolean                                     | 是否有边框              | true       |