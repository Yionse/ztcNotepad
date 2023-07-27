# Form

- 表单域

- Form是一个顶层组件，要使用表单域需要在其组件中，嵌套一个Form.Item组件，然后在Form.Item组件中书写想要的内容

- ```tsx
  <Form name="新建活动">
        <Form.Item label="活动名称" name="activityName" required>
          <Input></Input>
        </Form.Item>
  </Form>
  ```
  
- Form.item永远是纯粹的，当在Item之外出现了别的结构可能出现问题

## 属性值列表

- initialValues	表单的默认值，只有初始化和重置时生效
- 