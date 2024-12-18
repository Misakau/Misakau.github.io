---
title: 'peft和pytorch不兼容'
date: 2024-12-18
permalink: /posts/2024/12/peft_and_pytorch/
tags:
  - env
  - pytorch
  - peft
---
最近在跑[LoRA微调Stable Diffusion](https://huggingface.co/blog/lora)的时候，发现程序在最后加载LoRA权重时报错：

```bash
  File "/anaconda3/envs/lora/lib/python3.10/site-packages/peft/utils/save_and_load.py", line 445, in set_peft_model_state_dict
  load_result = model.load_state_dict(peft_model_state_dict, strict=False, assign=True)
  TypeError: Module.load_state_dict() got an unexpected keyword argument 'assign'
```

搜了一大圈之后才搞清楚原来`assign`关键字是在 [Pytorch 2.1.x](https://pytorch.org/docs/2.1/generated/torch.nn.Module.html#torch.nn.Module.load_state_dict) 新增的，而我自己的版本是 [Pytorch 2.0.x](https://pytorch.org/docs/2.0/generated/torch.nn.Module.html?highlight=nn+module+load_state_dict#torch.nn.Module.load_state_dict)，所以并没有这个参数...

解决方法是把`peft`版本降低到`peft==0.12.0`，这个版本是支持`Pytorch 2.0.x`的。 而从`peft==0.13.0`开始就引入了上述不兼容的代码。