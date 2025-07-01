# 如何新增或修改模型 & 压缩算法

量化目前支持FP8、INT8、INT4、


```{eval-rst}
.. tabs::

   .. tab:: Python
      .. code-block:: python
         :linenos:

         slim_config = {
            "global_config": global_config,
            "compress_config": compress_config,
        }
        self.compressor = CompressorFactory.create(
            "PTQ", self.slim_model, slim_config=slim_config
        )

```


## 新增模型