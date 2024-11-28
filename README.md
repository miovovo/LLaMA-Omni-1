**LLama-Omni Training Code Recurrence（llama-omni训练代码复现）**

1.根据LLama-omni给的方法进行环境安装(https://github.com/ictnlp/LLaMA-Omni)
2.wavs下是根据论文方法生成的100条数据（指令相同，模型用的Qwen，其他保持一致），用于阶段一和阶段二的训练
3.whisper模型放到models下（需要自己下载）；一阶段我用的是1B模型（Llama-3.2-1B-Instruct），下载后放到当前目录下，记得修改config.json的关于whisper配置，否则会报错；如果跑的起8B，可直接用原论文的权重，或原生8B进行修改。
4.vocoder是音频生成模块的权重（已包含）。
5.二阶段的数据在omni_speech/infer/gen_answer_data/answer.json，用wavs下的question音频生成的回复生成的token
6.两个阶段的精度用的都是bf16，loss可以正常下降，单卡3090，一阶段由于是1B可以多卡跑起来，二阶段由于设备有限，只在单卡上跑了跑，可以收敛到2。fp16会有loss nan的问题。
7.启动方法：一阶段bash /mnt/nanwu/project/LLaMA-Omni/omni_speech/train/run.sh
          二阶段  python /mnt/nanwu/project/LLaMA-Omni/omni_speech/train/stage2.py    

PS：感谢LLama-Omni的工作！也感谢另一位小伙伴@EDGSCOUT-li，我们一起复现这篇论文的训练过程，由于我们打算去复现freeze_omni(https://github.com/VITA-MLLM/Freeze-Omni)去了，该项目暂时做到这里，如果有issue看到后会及时回复。