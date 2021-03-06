.. _cn_api_fluid_name_scope:

name_scope
-------------------------------

.. py:function:: paddle.fluid.name_scope(prefix=None)


该OP为operators生成不同的命名空间。此OP用于调试和可视化，不建议用在其它方面。


参数：
  - **prefix** (str，可选) - 名称前缀。默认值为None。

**示例代码**

.. code-block:: python
          
     import paddle.fluid as fluid
     with fluid.name_scope("s1"):
        a = fluid.layers.data(name='data', shape=[1], dtype='int32')
        b = a + 1
        with fluid.name_scope("s2"):
           c = b * 1
        with fluid.name_scope("s3"):
           d = c / 1
     with fluid.name_scope("s1"):
           f = fluid.layers.pow(d, 2.0)
     with fluid.name_scope("s4"):
           g = f - 1

     # 没有指定的话默认OP在default main program中。
     for op in fluid.default_main_program().block(0).ops:
         # elementwise_add在/s1/中创建
         if op.type == 'elementwise_add':
             self.assertEqual(op.desc.attr("op_namescope"), '/s1/')
         # elementwise_mul在/s1/s2中创建
         elif op.type == 'elementwise_mul':
             self.assertEqual(op.desc.attr("op_namescope"), '/s1/s2/')
         # elementwise_div在/s1/s3中创建
         elif op.type == 'elementwise_div':
             self.assertEqual(op.desc.attr("op_namescope"), '/s1/s3/')
         # elementwise_sum在/s4/中创建
         elif op.type == 'elementwise_sub':
             self.assertEqual(op.desc.attr("op_namescope"), '/s4/')
         # pow在/s1_1/中创建
         elif op.type == 'pow':
             self.assertEqual(op.desc.attr("op_namescope"), '/s1_1/')


