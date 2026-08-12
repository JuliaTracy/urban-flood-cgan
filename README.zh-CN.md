# 城市内涝快速预测模型发布说明

本仓库对应论文：

> Li, R., Huang, Z., Dong, Y., Hu, Q., Dong, X., & Zhong, L. (2026). A mechanism-data hybrid pretraining-finetuning framework for rapid and reliable urban pluvial flooding prediction. *Water Research*, 305, 126491. https://doi.org/10.1016/j.watres.2026.126491

## 发布范围

本 Git 仓库仅公开说明文档。经审核的下载包只包含训练后的模型文件、模型接口说明和校验值，不包含训练或推理源码、水深提取源码、原始观测、机理模型结果、社交媒体记录、GIS 数据、坐标、密钥或研究区配置。

作者保存的两个原始 PyTorch checkpoint 为：

- `pretrain_early_stop_20250903_0301.pth`：机理信息预训练后的模型；
- `finetune_early_stop_20250902_1620_1.pth`：观测数据微调后的模型。

`.pth` 为依赖私有网络结构的 `state_dict`。在不公开架构源码的情况下，使用者应优先使用审批下载包中的 TorchScript 生成器模型（`*.ts`）。公开输入输出接口见 [MODEL_USAGE.md](MODEL_USAGE.md)。

## 模型申请与信息登记

公开 GitHub 仓库无法在 clone 或 Download ZIP 前强制填写问卷，也不会向仓库所有者提供下载者的身份、行业、地区或单位。因此，模型二进制文件不直接放在 Git 仓库中，而采用“邮件登记—审核—发送下载方式”的流程。

请阅读 [PRIVACY_NOTICE.md](PRIVACY_NOTICE.md)，填写 [ACCESS_REQUEST_TEMPLATE.md](ACCESS_REQUEST_TEMPLATE.md)，并使用机构或工作邮箱发送给通讯作者之一：

- 董欣：`dongxin@tsinghua.edu.cn`
- 钟立金：`zhonglijin@huanding.org`

登记内容包括姓名与身份/角色、行业、国家或地区、单位、用途及条款确认。请勿提交身份证件号码、家庭住址等敏感个人信息。申请不会自动获批；审核通过后，通讯作者将提供当前下载地址及适用条款。

## 源码申请

源码不公开发布。如科研工作确有需要，可向通讯作者说明研究目的、所需模块及使用方式。源码申请将逐案评估，并可能需要签署额外条款或取得机构批准。

## 重要限制

该模型属于科研成果，不是业务化预警系统。不得将其作为应急响应、公共安全、工程设计、保险或监管决策的唯一依据。
