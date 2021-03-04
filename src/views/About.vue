<template>
  <div class="about">
    <div class="option-btn">
      <a-button type="primary" class="add-btn" @click="showModal()">
        Add
      </a-button>
    </div>
    <a-table :columns="columns" :data-source="data">
      <template #name="{ text }">
        <a>{{ text }}</a>
      </template>
      <template #customTitle>
        <span><smile-outlined /> Name</span>
      </template>
      <template #tags="{ text: tags }">
        <span>
          <a-tag
            v-for="tag in tags"
            :key="tag"
            :color="tag === 'loser' ? 'volcano' : tag.length > 5 ? 'geekblue' : 'green'"
          >
            {{ tag.toUpperCase() }}
          </a-tag>
        </span>
      </template>
      <template #action="{ record }">
        <span>
          <a>Edit 一 {{ record.name }}</a>
          <a-divider type="vertical" />
          <a>Delete</a>
        </span>
      </template>
    </a-table>
    <a-modal
      :title="title"
      v-model:visible="visible"
      @ok="handleOk"
    >
      <a-form :label-col="labelCol" :wrapper-col="wrapperCol">
        <a-form-item label="Activity name" v-bind="validateInfos.name">
          <a-input v-model:value="modelRef.name" />
        </a-form-item>
        <a-form-item label="Activity age" v-bind="validateInfos.age">
          <a-input v-model:value="modelRef.age" />
        </a-form-item>
        <a-form-item label="Activity tags" v-bind="validateInfos.tags">
          <a-checkbox-group v-model:value="modelRef.tags">
            <a-checkbox value="1" name="tag">
              NICE
            </a-checkbox>
            <a-checkbox value="2" name="tag">
              LOSER
            </a-checkbox>
            <a-checkbox value="3" name="tag">
              TEACHER
            </a-checkbox>
          </a-checkbox-group>
        </a-form-item>
      </a-form>
    </a-modal>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref, toRefs } from 'vue';
import { SmileOutlined } from '@ant-design/icons-vue'
import { useForm } from '@ant-design-vue/use';

const columns = [
  {
    dataIndex: 'name',
    key: 'name',
    slots: { title: 'customTitle', customRender: 'name' }
  },
  {
    title: 'Age',
    dataIndex: 'age',
    key: 'age'
  },
  {
    title: 'Address',
    dataIndex: 'address',
    key: 'address'
  },
  {
    title: 'Tags',
    key: 'tags',
    dataIndex: 'tags',
    slots: { customRender: 'tags' }
  },
  {
    title: 'Action',
    key: 'action',
    slots: { customRender: 'action' }
  }
];

const data = [
  {
    key: '1',
    name: 'John Brown',
    age: 32,
    address: 'New York No. 1 Lake Park',
    tags: ['nice', 'developer']
  },
  {
    key: '2',
    name: 'Jim Green',
    age: 42,
    address: 'London No. 1 Lake Park',
    tags: ['loser']
  },
  {
    key: '3',
    name: 'Joe Black',
    age: 32,
    address: 'Sidney No. 1 Lake Park',
    tags: ['cool', 'teacher']
  }
];
export default defineComponent({
  name: 'About',
  components: {
    SmileOutlined
  },
  setup(props, context) {
    const state = reactive({
      columns,
      data
    })
    const modelRef = reactive({
      name: '',
      age: undefined,
      tags: []
    });
    const visible = ref(false)
    const title = ref('Add User')
    const showModal = function () {
      visible.value = true
    }
    const rulesRef = reactive({
      name: [
        {
          required: true,
          message: 'Please input name',
        },
      ],
      age: [
        {
          required: true,
          message: 'Please select age',
        },
      ],
      tags: [
        {
          required: true,
          message: 'Please select tags',
          type: 'array',
        },
      ]
    });
    const { resetFields, validate, validateInfos } = useForm(modelRef, rulesRef);
    const handleOk = (e: any) => {
      e.preventDefault();
      validate()
        .then(() => {
            console.log('success');
        })
        .catch(err => {
            console.log('error', err);
        });
    };
    return {
      ...toRefs(state),
      modelRef,
      visible,
      title,
      showModal,
      labelCol: { span: 6 },
      wrapperCol: { span: 14 },
      validateInfos,
      resetFields,
      handleOk
    }
  }
});
</script>
<style lang="scss">
  .option-btn {
    text-align: right;
    margin-bottom: 10px;
  }
</style>
