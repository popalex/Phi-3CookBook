# 在 Azure AI Foundry 中评估微调后的 Phi-3 / Phi-3.5 模型，重点关注微软的负责任 AI 原则

这个端到端（E2E）示例基于微软技术社区的指南“[在 Azure AI Foundry 中评估微调后的 Phi-3 / 3.5 模型，重点关注微软的负责任 AI](https://techcommunity.microsoft.com/t5/educator-developer-blog/evaluate-fine-tuned-phi-3-3-5-models-in-azure-ai-studio-focusing/ba-p/4227850?WT.mc_id=aiml-137032-kinfeylo)”。

## 概述

### 如何在 Azure AI Foundry 中评估微调后的 Phi-3 / Phi-3.5 模型的安全性和性能？

微调模型有时会导致意外或不期望的响应。为了确保模型保持安全和有效，评估模型生成有害内容的潜力及其产生准确、相关和连贯响应的能力是非常重要的。在本教程中，您将学习如何评估集成了 Prompt flow 的 Azure AI Foundry 中的微调 Phi-3 / Phi-3.5 模型的安全性和性能。

以下是 Azure AI Foundry 的评估过程。

![教程架构图。](../../../../translated_images/architecture.1eb9d143d0771c6065f16c0f66a9eb233f466cdf9db0b0afe11adcbd57eb06ce.zh.png)

*图片来源: [生成 AI 应用的评估](https://learn.microsoft.com/azure/ai-studio/concepts/evaluation-approach-gen-ai?wt.mc_id%3Dstudentamb_279723)*

> [!NOTE]
>
> 有关 Phi-3 / Phi-3.5 的更多详细信息和其他资源，请访问 [Phi-3CookBook](https://github.com/microsoft/Phi-3CookBook?wt.mc_id=studentamb_279723)。

### 前提条件

- [Python](https://www.python.org/downloads)
- [Azure 订阅](https://azure.microsoft.com/free?wt.mc_id=studentamb_279723)
- [Visual Studio Code](https://code.visualstudio.com)
- 微调后的 Phi-3 / Phi-3.5 模型

### 目录

1. [**场景 1: 介绍 Azure AI Foundry 的 Prompt flow 评估**](../../../../md/06.E2ESamples)

    - [安全性评估介绍](../../../../md/06.E2ESamples)
    - [性能评估介绍](../../../../md/06.E2ESamples)

1. [**场景 2: 在 Azure AI Foundry 中评估 Phi-3 / Phi-3.5 模型**](../../../../md/06.E2ESamples)

    - [开始之前](../../../../md/06.E2ESamples)
    - [部署 Azure OpenAI 以评估 Phi-3 / Phi-3.5 模型](../../../../md/06.E2ESamples)
    - [使用 Azure AI Foundry 的 Prompt flow 评估微调后的 Phi-3 / Phi-3.5 模型](../../../../md/06.E2ESamples)

1. [恭喜！](../../../../md/06.E2ESamples)

## **场景 1: 介绍 Azure AI Foundry 的 Prompt flow 评估**

### 安全性评估介绍

为了确保您的 AI 模型符合伦理和安全性，评估其是否符合微软的负责任 AI 原则是至关重要的。在 Azure AI Foundry 中，安全性评估允许您评估模型对越狱攻击的脆弱性及其生成有害内容的潜力，这与这些原则直接相关。

![安全性评估。](../../../../translated_images/safety-evaluation.5ad906c4618e4c8736fdeeff54a52dac8ae6d0756b15824e430177fba7b6a8b4.zh.png)

*图片来源: [生成 AI 应用的评估](https://learn.microsoft.com/azure/ai-studio/concepts/evaluation-approach-gen-ai?wt.mc_id%3Dstudentamb_279723)*

#### 微软的负责任 AI 原则

在开始技术步骤之前，了解微软的负责任 AI 原则是很重要的，这是一种指导负责任开发、部署和操作 AI 系统的伦理框架。这些原则指导 AI 系统的负责任设计、开发和部署，确保 AI 技术的构建方式是公平、透明和包容的。这些原则是评估 AI 模型安全性的基础。

微软的负责任 AI 原则包括：

- **公平和包容性**: AI 系统应公平对待每个人，避免对类似情况的群体产生不同的影响。例如，当 AI 系统提供医疗建议、贷款申请或就业指导时，它们应对具有相似症状、财务状况或专业资格的每个人做出相同的推荐。

- **可靠性和安全性**: 为了建立信任，AI 系统必须可靠、安全和一致地运行。这些系统应能够按原设计运行，安全地响应意外情况，并抵御有害操纵。它们的行为和能够处理的条件范围反映了开发人员在设计和测试期间预见的各种情况和环境。

- **透明性**: 当 AI 系统帮助做出对人们生活有重大影响的决策时，人们了解这些决策是如何做出的至关重要。例如，银行可能使用 AI 系统来决定一个人是否有信用。一家公司可能使用 AI 系统来确定最合格的候选人。

- **隐私和安全**: 随着 AI 的普及，保护隐私和确保个人及商业信息的安全变得越来越重要和复杂。由于数据访问对于 AI 系统做出准确和知情的预测和决策至关重要，因此在 AI 中隐私和数据安全需要特别关注。

- **问责**: 设计和部署 AI 系统的人必须对其系统的运行负责。组织应借鉴行业标准来制定问责规范。这些规范可以确保 AI 系统不是对任何影响人们生活的决策的最终权威。它们还可以确保人类保持对高度自主的 AI 系统的有意义控制。

![责任中心。](../../../../translated_images/responsibleai2.ae6f996d95dcc46b3b3087c0e4f4f4b74c64e961083009ecca7a0a3998b3f716.zh.png)

*图片来源: [什么是负责任 AI？](https://learn.microsoft.com/azure/machine-learning/concept-responsible-ai?view=azureml-api-2&viewFallbackFrom=azureml-api-2%253fwt.mc_id%3Dstudentamb_279723)*

> [!NOTE]
> 要了解更多关于微软的负责任 AI 原则，请访问 [什么是负责任 AI？](https://learn.microsoft.com/azure/machine-learning/concept-responsible-ai?view=azureml-api-2?wt.mc_id=studentamb_279723)。

#### 安全性指标

在本教程中，您将使用 Azure AI Foundry 的安全性指标来评估微调后的 Phi-3 模型的安全性。这些指标帮助您评估模型生成有害内容的潜力及其对越狱攻击的脆弱性。安全性指标包括：

- **自我伤害相关内容**: 评估模型是否有生成自我伤害相关内容的倾向。
- **仇恨和不公平内容**: 评估模型是否有生成仇恨或不公平内容的倾向。
- **暴力内容**: 评估模型是否有生成暴力内容的倾向。
- **性内容**: 评估模型是否有生成不适当性内容的倾向。

评估这些方面确保 AI 模型不会生成有害或冒犯性内容，使其符合社会价值观和监管标准。

![基于安全性评估。](../../../../translated_images/evaluate-based-on-safety.63d79ac01570713002d5d858bfbb8f4d7295557ae8829d0c379485d67a3fcd1c.zh.png)

### 性能评估介绍

为了确保您的 AI 模型按预期执行，评估其性能指标是很重要的。在 Azure AI Foundry 中，性能评估允许您评估模型生成准确、相关和连贯响应的有效性。

![性能评估。](../../../../translated_images/performance-evaluation.259c44647c74a80761cdcbaab2202142f99a5a0d28326c8a1eb6dc2765f5ef77.zh.png)

*图片来源: [生成 AI 应用的评估](https://learn.microsoft.com/azure/ai-studio/concepts/evaluation-approach-gen-ai?wt.mc_id%3Dstudentamb_279723)*

#### 性能指标

在本教程中，您将使用 Azure AI Foundry 的性能指标来评估微调后的 Phi-3 / Phi-3.5 模型的性能。这些指标帮助您评估模型生成准确、相关和连贯响应的有效性。性能指标包括：

- **基础性**: 评估生成的答案与输入源信息的对齐程度。
- **相关性**: 评估生成的响应与给定问题的相关性。
- **连贯性**: 评估生成文本的流畅度、自然度和类似人类语言的程度。
- **流利度**: 评估生成文本的语言熟练度。
- **GPT 相似度**: 将生成的响应与真实答案进行相似性比较。
- **F1 分数**: 计算生成响应与源数据之间的共享词汇比例。

这些指标帮助您评估模型生成准确、相关和连贯响应的有效性。

![基于性能评估。](../../../../translated_images/evaluate-based-on-performance.33ccf1c52f210af8e76d9cab863716d3f67db3d765254371a30136cc8f852b37.zh.png)

## **场景 2: 在 Azure AI Foundry 中评估 Phi-3 / Phi-3.5 模型**

### 开始之前

本教程是之前博客文章的后续内容，“[微调并集成自定义 Phi-3 模型与 Prompt Flow：逐步指南](https://techcommunity.microsoft.com/t5/educator-developer-blog/fine-tune-and-integrate-custom-phi-3-models-with-prompt-flow/ba-p/4178612?wt.mc_id=studentamb_279723)”和“[在 Azure AI Foundry 中微调并集成自定义 Phi-3 模型与 Prompt Flow](https://techcommunity.microsoft.com/t5/educator-developer-blog/fine-tune-and-integrate-custom-phi-3-models-with-prompt-flow-in/ba-p/4191726?wt.mc_id=studentamb_279723)”。在这些文章中，我们详细介绍了在 Azure AI Foundry 中微调 Phi-3 / Phi-3.5 模型并将其与 Prompt flow 集成的过程。

在本教程中，您将部署一个 Azure OpenAI 模型作为评估器，并使用它来评估您的微调后的 Phi-3 / Phi-3.5 模型。

在开始本教程之前，请确保您具备以下前提条件，如前面的教程中所述：

1. 准备好评估微调后的 Phi-3 / Phi-3.5 模型的数据集。
1. 已微调并部署到 Azure 机器学习的 Phi-3 / Phi-3.5 模型。
1. 在 Azure AI Foundry 中与微调后的 Phi-3 / Phi-3.5 模型集成的 Prompt flow。

> [!NOTE]
> 您将使用 *test_data.jsonl* 文件，该文件位于之前博客文章中下载的 **ULTRACHAT_200k** 数据集的 data 文件夹中，作为评估微调后的 Phi-3 / Phi-3.5 模型的数据集。

#### 在 Azure AI Foundry 中将自定义 Phi-3 / Phi-3.5 模型与 Prompt flow 集成（代码优先方法）

> [!NOTE]
> 如果您按照“[在 Azure AI Foundry 中微调并集成自定义 Phi-3 模型与 Prompt Flow](https://techcommunity.microsoft.com/t5/educator-developer-blog/fine-tune-and-integrate-custom-phi-3-models-with-prompt-flow-in/ba-p/4191726?wt.mc_id=studentamb_279723)”中描述的低代码方法操作，可以跳过此练习并继续下一个。
> 然而，如果您按照“[微调并集成自定义 Phi-3 模型与 Prompt Flow：逐步指南](https://techcommunity.microsoft.com/t5/educator-developer-blog/fine-tune-and-integrate-custom-phi-3-models-with-prompt-flow/ba-p/4178612?wt.mc_id=studentamb_279723)”中描述的代码优先方法微调和部署了您的 Phi-3 / Phi-3.5 模型，则连接您的模型到 Prompt flow 的过程稍有不同。您将在此练习中学习这一过程。

要继续操作，您需要将微调后的 Phi-3 / Phi-3.5 模型集成到 Azure AI Foundry 中的 Prompt flow。

#### 创建 Azure AI Foundry Hub

在创建项目之前，您需要创建一个 Hub。Hub 类似于资源组，允许您在 Azure AI Foundry 中组织和管理多个项目。

1. 登录 [Azure AI Foundry](https://ai.azure.com/?wt.mc_id=studentamb_279723)。

1. 从左侧选项卡中选择 **所有 hubs**。

1. 从导航菜单中选择 **+ 新建 hub**。

    ![创建 hub。](../../../../translated_images/create-hub.8d452311ef5b4b9188df89f7cff7797c67ec8f391282235b19b988e167f3e920.zh.png)

1. 执行以下任务：

    - 输入 **Hub 名称**。它必须是唯一值。
    - 选择您的 Azure **订阅**。
    - 选择要使用的 **资源组**（如有需要，创建一个新的）。
    - 选择您要使用的 **位置**。
    - 选择要使用的 **连接 Azure AI 服务**（如有需要，创建一个新的）。
    - 选择 **连接 Azure AI 搜索** 到 **跳过连接**。
![填充中心。](../../../../translated_images/fill-hub.8b19834866ef631a2fd031584c77b78c0438a385bdee8f981361b14bbc46f5e1.zh.png)

1. 选择 **Next**。

#### 创建 Azure AI Foundry 项目

1. 在你创建的中心中，从左侧标签选择 **All projects**。

1. 从导航菜单中选择 **+ New project**。

    ![选择新项目。](../../../../translated_images/select-new-project.1a925c25ca3bc47b2b16feefc5a76f5c29e211ac464ea5c3cdfe389868d87da7.zh.png)

1. 输入 **Project name**。它必须是唯一的值。

    ![创建项目。](../../../../translated_images/create-project.ff239752937343b4cb38156740ebda92bc1d8b16299183c245f3e3661432db31.zh.png)

1. 选择 **Create a project**。

#### 为微调的 Phi-3 / Phi-3.5 模型添加自定义连接

要将你的自定义 Phi-3 / Phi-3.5 模型与 Prompt flow 集成，你需要在自定义连接中保存模型的端点和密钥。此设置可确保在 Prompt flow 中访问你的自定义 Phi-3 / Phi-3.5 模型。

#### 设置微调的 Phi-3 / Phi-3.5 模型的 API 密钥和端点 URI

1. 访问 [Azure ML Studio](https://ml.azure.com/home?wt.mc_id=studentamb_279723)。

1. 导航到你创建的 Azure 机器学习工作区。

1. 从左侧标签选择 **Endpoints**。

    ![选择端点。](../../../../translated_images/select-endpoints.3027748aed379990fd8ee9bf27f70ff11918955bb1a10269e2f34f6931b26955.zh.png)

1. 选择你创建的端点。

    ![选择已创建的端点。](../../../../translated_images/select-endpoint-created.560a5cadfbbb58b66abb2fb61b4450b22060371910af1b45c358bde548234ebc.zh.png)

1. 从导航菜单中选择 **Consume**。

1. 复制你的 **REST endpoint** 和 **Primary key**。

    ![复制 API 密钥和端点 URI。](../../../../translated_images/copy-endpoint-key.56de01742992f2402d57139849304b5b049fb468fb38abc7982b7dfc311daf9e.zh.png)

#### 添加自定义连接

1. 访问 [Azure AI Foundry](https://ai.azure.com/?wt.mc_id=studentamb_279723)。

1. 导航到你创建的 Azure AI Foundry 项目。

1. 在你创建的项目中，从左侧标签选择 **Settings**。

1. 选择 **+ New connection**。

    ![选择新连接。](../../../../translated_images/select-new-connection.a1ff72172d07054308a3fc7b7b86e25e9ca1c879f0a382b9a2be2c80bb2ebcc5.zh.png)

1. 从导航菜单中选择 **Custom keys**。

    ![选择自定义密钥。](../../../../translated_images/select-custom-keys.b17eb3b15eae28126a4eeda8f53396b9a6f773745f2dd68c464252575a77b5d3.zh.png)

1. 执行以下任务：

    - 选择 **+ Add key value pairs**。
    - 对于密钥名称，输入 **endpoint** 并将从 Azure ML Studio 复制的端点粘贴到值字段中。
    - 再次选择 **+ Add key value pairs**。
    - 对于密钥名称，输入 **key** 并将从 Azure ML Studio 复制的密钥粘贴到值字段中。
    - 添加密钥后，选择 **is secret** 以防止密钥暴露。

    ![添加连接。](../../../../translated_images/add-connection.c01c94851c9eece708711456a4853355b9532b0cb1f970e24f165e7e1d6c0a03.zh.png)

1. 选择 **Add connection**。

#### 创建 Prompt flow

你已经在 Azure AI Foundry 中添加了自定义连接。现在，让我们使用以下步骤创建一个 Prompt flow。然后，你将把这个 Prompt flow 连接到自定义连接，以便在 Prompt flow 中使用微调模型。

1. 导航到你创建的 Azure AI Foundry 项目。

1. 从左侧标签选择 **Prompt flow**。

1. 从导航菜单中选择 **+ Create**。

    ![选择 Promptflow。](../../../../translated_images/select-promptflow.766ad0f2403e2bd6f374bee40a607321e3e2b416629063acf3204a983fb4bcaa.zh.png)

1. 从导航菜单中选择 **Chat flow**。

    ![选择聊天流。](../../../../translated_images/select-flow-type.0e18b84032da1200e48a702e5140c1775b1eb6de9cf222c6a8007840fa278e97.zh.png)

1. 输入要使用的 **Folder name**。

    ![选择聊天流。](../../../../translated_images/enter-name.43919b211b1e8e0184536dc09184190e7e8c56bf960d4b5601443efddc757db4.zh.png)

1. 选择 **Create**。

#### 设置 Prompt flow 与你的自定义 Phi-3 / Phi-3.5 模型聊天

你需要将微调的 Phi-3 / Phi-3.5 模型集成到一个 Prompt flow 中。然而，现有的 Prompt flow 并不适合此目的。因此，你必须重新设计 Prompt flow 以实现自定义模型的集成。

1. 在 Prompt flow 中，执行以下任务以重建现有流程：

    - 选择 **Raw file mode**。
    - 删除 *flow.dag.yml* 文件中的所有现有代码。
    - 将以下代码添加到 *flow.dag.yml*。

        ```yml
        inputs:
          input_data:
            type: string
            default: "Who founded Microsoft?"

        outputs:
          answer:
            type: string
            reference: ${integrate_with_promptflow.output}

        nodes:
        - name: integrate_with_promptflow
          type: python
          source:
            type: code
            path: integrate_with_promptflow.py
          inputs:
            input_data: ${inputs.input_data}
        ```

    - 选择 **Save**。

    ![选择原始文件模式。](../../../../translated_images/select-raw-file-mode.2084d7f905df40f69cc5ebe48efa2318e92fd069358625a92993ef614f189d84.zh.png)

1. 将以下代码添加到 *integrate_with_promptflow.py* 以在 Prompt flow 中使用自定义 Phi-3 / Phi-3.5 模型。

    ```python
    import logging
    import requests
    from promptflow import tool
    from promptflow.connections import CustomConnection

    # Logging setup
    logging.basicConfig(
        format="%(asctime)s - %(levelname)s - %(name)s - %(message)s",
        datefmt="%Y-%m-%d %H:%M:%S",
        level=logging.DEBUG
    )
    logger = logging.getLogger(__name__)

    def query_phi3_model(input_data: str, connection: CustomConnection) -> str:
        """
        Send a request to the Phi-3 / Phi-3.5 model endpoint with the given input data using Custom Connection.
        """

        # "connection" is the name of the Custom Connection, "endpoint", "key" are the keys in the Custom Connection
        endpoint_url = connection.endpoint
        api_key = connection.key

        headers = {
            "Content-Type": "application/json",
            "Authorization": f"Bearer {api_key}"
        }
    data = {
        "input_data": [input_data],
        "params": {
            "temperature": 0.7,
            "max_new_tokens": 128,
            "do_sample": True,
            "return_full_text": True
            }
        }
        try:
            response = requests.post(endpoint_url, json=data, headers=headers)
            response.raise_for_status()
            
            # Log the full JSON response
            logger.debug(f"Full JSON response: {response.json()}")

            result = response.json()["output"]
            logger.info("Successfully received response from Azure ML Endpoint.")
            return result
        except requests.exceptions.RequestException as e:
            logger.error(f"Error querying Azure ML Endpoint: {e}")
            raise

    @tool
    def my_python_tool(input_data: str, connection: CustomConnection) -> str:
        """
        Tool function to process input data and query the Phi-3 / Phi-3.5 model.
        """
        return query_phi3_model(input_data, connection)

    ```

    ![粘贴 Prompt flow 代码。](../../../../translated_images/paste-promptflow-code.667abbdf848fd03153828c0aa70dde58a851e09b1ba4c69bc6f686d2005656aa.zh.png)

> [!NOTE]
> 有关在 Azure AI Foundry 中使用 Prompt flow 的详细信息，可以参考 [Prompt flow in Azure AI Foundry](https://learn.microsoft.com/azure/ai-studio/how-to/prompt-flow)。

1. 选择 **Chat input**、**Chat output** 以启用与模型的聊天。

    ![选择输入输出。](../../../../translated_images/select-input-output.d4803eae144b03579db4a23def15c68bb50527017cdc4aa9f72c8679588a0f4c.zh.png)

1. 现在你已经准备好与自定义的 Phi-3 / Phi-3.5 模型聊天了。在下一个练习中，你将学习如何启动 Prompt flow 并使用它与微调的 Phi-3 / Phi-3.5 模型聊天。

> [!NOTE]
>
> 重建的流程应如下图所示：
>
> ![流程示例](../../../../translated_images/graph-example.5b309021deca5b503270e50888da4784256730c210ed757f799f9bff0a12bb19.zh.png)
>

#### 启动 Prompt flow

1. 选择 **Start compute sessions** 以启动 Prompt flow。

    ![启动计算会话。](../../../../translated_images/start-compute-session.75300f481218ca70eae80304255956c71a9b6be31a18b43264118c19c0b1a016.zh.png)

1. 选择 **Validate and parse input** 以更新参数。

    ![验证输入。](../../../../translated_images/validate-input.a6c90e55ce6117ea44e2acd88b8a20bc31136bf6c574b0a6c9217867201025c9.zh.png)

1. 选择自定义连接的 **connection** 的 **Value**。例如，*connection*。

    ![连接。](../../../../translated_images/select-connection.6573a1269969a14c83c397334051f71057ec9a243e95ea1b849483bd7481ff6a.zh.png)

#### 与自定义 Phi-3 / Phi-3.5 模型聊天

1. 选择 **Chat**。

    ![选择聊天。](../../../../translated_images/select-chat.25eab3d62b0a6c4960f0ae1b5d6efd6b5847cc20d468fd28cb1f0d656936cc22.zh.png)

1. 以下是结果示例：现在你可以与自定义的 Phi-3 / Phi-3.5 模型聊天了。建议基于用于微调的数据提问。

    ![与 Prompt flow 聊天。](../../../../translated_images/chat-with-promptflow.105b5a3b70ff64725c1d4cd2e9c6b55301219c7d68c9bec59106a49fb8bf40f2.zh.png)

### 部署 Azure OpenAI 以评估 Phi-3 / Phi-3.5 模型

要在 Azure AI Foundry 中评估 Phi-3 / Phi-3.5 模型，你需要部署一个 Azure OpenAI 模型。该模型将用于评估 Phi-3 / Phi-3.5 模型的性能。

#### 部署 Azure OpenAI

1. 登录 [Azure AI Foundry](https://ai.azure.com/?wt.mc_id=studentamb_279723)。

1. 导航到你创建的 Azure AI Foundry 项目。

    ![选择项目。](../../../../translated_images/select-project-created.7b3c97c3883c3a95d547173b41e579cd748df940749c3d9616fe03ef46a3eda2.zh.png)

1. 在你创建的项目中，从左侧标签选择 **Deployments**。

1. 从导航菜单中选择 **+ Deploy model**。

1. 选择 **Deploy base model**。

    ![选择部署。](../../../../translated_images/deploy-openai-model.f09a74e1f976b52f85fdef711372452b9faed270e9d015887e09f44b41bbc163.zh.png)

1. 选择你想使用的 Azure OpenAI 模型。例如，**gpt-4o**。

    ![选择要使用的 Azure OpenAI 模型。](../../../../translated_images/select-openai-model.29fbbd802d6a9aa941097ae80a0ffe256239e636190b7c1e49f28d3d66143a0d.zh.png)

1. 选择 **Confirm**。

### 使用 Azure AI Foundry 的 Prompt flow 评估微调的 Phi-3 / Phi-3.5 模型

### 开始新的评估

1. 访问 [Azure AI Foundry](https://ai.azure.com/?wt.mc_id=studentamb_279723)。

1. 导航到你创建的 Azure AI Foundry 项目。

    ![选择项目。](../../../../translated_images/select-project-created.7b3c97c3883c3a95d547173b41e579cd748df940749c3d9616fe03ef46a3eda2.zh.png)

1. 在你创建的项目中，从左侧标签选择 **Evaluation**。

1. 从导航菜单中选择 **+ New evaluation**。
![选择评估。](../../../../translated_images/select-evaluation.7d8a8228ebdf3f3b46bf5cf6ab5bdcb4565132ba6b28126d9757aaf3abade725.zh.png)

1. 选择 **Prompt flow** 评估。

    ![选择 Prompt flow 评估。](../../../../translated_images/promptflow-evaluation.ff4265fafd69c7f5ded1b5275cacecbd5f3272a6358c1f784f62e64bcb9e949f.zh.png)

1. 执行以下任务：

    - 输入评估名称。它必须是唯一的值。
    - 选择 **Question and answer without context** 作为任务类型。因为本教程中使用的 **UlTRACHAT_200k** 数据集不包含上下文。
    - 选择你想要评估的 prompt flow。

    ![Prompt flow 评估。](../../../../translated_images/evaluation-setting1.d3b22a8343f8265807e2b507fa7eec9d86a9cefd4a67bc39ba2022d572f13e1d.zh.png)

1. 选择 **Next**。

1. 执行以下任务：

    - 选择 **Add your dataset** 上传数据集。例如，你可以上传测试数据集文件，比如 *test_data.json1*，它包含在你下载 **ULTRACHAT_200k** 数据集时。
    - 选择与你的数据集匹配的 **Dataset column**。例如，如果你使用的是 **ULTRACHAT_200k** 数据集，选择 **${data.prompt}** 作为数据集列。

    ![Prompt flow 评估。](../../../../translated_images/evaluation-setting2.32749fa51bc4fdb32f75dfd09b96bee74454c51ce3084bcc6f95b7286177a657.zh.png)

1. 选择 **Next**。

1. 执行以下任务来配置性能和质量指标：

    - 选择你想使用的性能和质量指标。
    - 选择你创建的用于评估的 Azure OpenAI 模型。例如，选择 **gpt-4o**。

    ![Prompt flow 评估。](../../../../translated_images/evaluation-setting3-1.db76b89d94911c84ece6ce793593a4289278e1b24520e38ecd8372f4b9dc1167.zh.png)

1. 执行以下任务来配置风险和安全指标：

    - 选择你想使用的风险和安全指标。
    - 选择用于计算缺陷率的阈值。例如，选择 **Medium**。
    - 对于 **question**，选择 **Data source** 为 **{$data.prompt}**。
    - 对于 **answer**，选择 **Data source** 为 **{$run.outputs.answer}**。
    - 对于 **ground_truth**，选择 **Data source** 为 **{$data.message}**。

    ![Prompt flow 评估。](../../../../translated_images/evaluation-setting3-2.eb9892654970af140ebb74fcc99e06dad7eca3d38365e3f2cbe90101392f41ee.zh.png)

1. 选择 **Next**。

1. 选择 **Submit** 开始评估。

1. 评估需要一些时间来完成。你可以在 **Evaluation** 选项卡中监控进度。

### 查看评估结果

> [!NOTE]
> 下面展示的结果旨在说明评估过程。在本教程中，我们使用了在相对较小的数据集上微调的模型，这可能会导致次优结果。实际结果可能会根据所使用的数据集的大小、质量和多样性以及模型的具体配置而显著不同。

评估完成后，你可以查看性能和安全指标的结果。

1. 性能和质量指标：

    - 评估模型在生成连贯、流畅和相关响应方面的效果。

    ![评估结果。](../../../../translated_images/evaluation-result-gpu.5b6e301e6d1af6044819f4d3c8443cbc44fb7db54ebce208b4288744ca25e6e8.zh.png)

1. 风险和安全指标：

    - 确保模型的输出是安全的，并符合负责任的 AI 原则，避免任何有害或冒犯性的内容。

    ![评估结果。](../../../../translated_images/evaluation-result-gpu-2.d867d40ee9dfebc40c878288b8dc8727721a2fec995904b1475c513f0960e8e0.zh.png)

1. 你可以向下滚动查看 **Detailed metrics result**。

    ![评估结果。](../../../../translated_images/detailed-metrics-result.6cf00c2b6026bb500ff758ee3047c20f600aab3878c892897e99e2e3a88fb002.zh.png)

1. 通过将你的自定义 Phi-3 / Phi-3.5 模型与性能和安全指标进行评估，你可以确认模型不仅有效，而且符合负责任的 AI 实践，使其准备好进行实际部署。

## 恭喜！

### 你已完成本教程

你已经成功评估了集成 Prompt flow 的 Azure AI Foundry 中微调的 Phi-3 模型。这是确保你的 AI 模型不仅性能良好，而且遵守 Microsoft 负责任的 AI 原则的重要一步，帮助你构建可信赖和可靠的 AI 应用程序。

![架构。](../../../../translated_images/architecture.1eb9d143d0771c6065f16c0f66a9eb233f466cdf9db0b0afe11adcbd57eb06ce.zh.png)

## 清理 Azure 资源

清理你的 Azure 资源以避免账户产生额外费用。前往 Azure 门户并删除以下资源：

- Azure 机器学习资源。
- Azure 机器学习模型端点。
- Azure AI Foundry 项目资源。
- Azure AI Foundry Prompt flow 资源。

### 后续步骤

#### 文档

- [microsoft/Phi-3CookBook](https://github.com/microsoft/Phi-3CookBook?wt.mc_id=studentamb_279723)
- [使用负责任的 AI 仪表板评估 AI 系统](https://learn.microsoft.com/azure/machine-learning/concept-responsible-ai-dashboard?view=azureml-api-2&source=recommendations?wt.mc_id=studentamb_279723)
- [生成式 AI 的评估和监控指标](https://learn.microsoft.com/azure/ai-studio/concepts/evaluation-metrics-built-in?tabs=definition?wt.mc_id=studentamb_279723)
- [Azure AI Foundry 文档](https://learn.microsoft.com/azure/ai-studio/?wt.mc_id=studentamb_279723)
- [Prompt flow 文档](https://microsoft.github.io/promptflow/?wt.mc_id=studentamb_279723)

#### 培训内容

- [Microsoft 负责任的 AI 方法简介](https://learn.microsoft.com/training/modules/introduction-to-microsofts-responsible-ai-approach/?source=recommendations?wt.mc_id=studentamb_279723)
- [Azure AI Foundry 简介](https://learn.microsoft.com/training/modules/introduction-to-azure-ai-studio/?wt.mc_id=studentamb_279723)

### 参考

- [microsoft/Phi-3CookBook](https://github.com/microsoft/Phi-3CookBook?wt.mc_id=studentamb_279723)
- [什么是负责任的 AI？](https://learn.microsoft.com/azure/machine-learning/concept-responsible-ai?view=azureml-api-2?wt.mc_id=studentamb_279723)
- [在 Azure AI 中宣布新工具，帮助你构建更安全和可信的生成式 AI 应用程序](https://azure.microsoft.com/blog/announcing-new-tools-in-azure-ai-to-help-you-build-more-secure-and-trustworthy-generative-ai-applications/?wt.mc_id=studentamb_279723)
- [生成式 AI 应用程序的评估](https://learn.microsoft.com/azure/ai-studio/concepts/evaluation-approach-gen-ai?wt.mc_id%3Dstudentamb_279723)

**免责声明**：
本文档使用基于机器的人工智能翻译服务进行翻译。尽管我们努力确保准确性，但请注意，自动翻译可能包含错误或不准确之处。应将原始语言的文档视为权威来源。对于关键信息，建议使用专业人工翻译。对于因使用此翻译而引起的任何误解或误读，我们不承担任何责任。