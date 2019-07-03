---
title: 用于 Python 的 Azure 认知服务模块
description: 用于 Python 的 Azure 认知服务模块参考
keywords: Azure, python, SDK, API, 认知服务
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 04/04/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 5a23a52414e70facd6feae3af3956a5131f6b5c4
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534345"
---
# <a name="azure-cognitive-services-modules-for-python"></a><span data-ttu-id="38658-104">用于 Python 的 Azure 认知服务模块</span><span class="sxs-lookup"><span data-stu-id="38658-104">Azure Cognitive Services modules for Python</span></span>

<span data-ttu-id="38658-105">使用用于 Python 的 Azure 认知服务模块向 Python 应用、网站和工具添加图像和人脸识别、语言分析以及搜索。</span><span class="sxs-lookup"><span data-stu-id="38658-105">Add image and face recognition, language analysis, and search to your Python apps, websites, and tools using the Azure Cognitive Services modules for Python.</span></span>

## <a name="vision-modules"></a><span data-ttu-id="38658-106">视觉模块</span><span class="sxs-lookup"><span data-stu-id="38658-106">Vision modules</span></span>

### <a name="computer-vision"></a><span data-ttu-id="38658-107">计算机视觉</span><span class="sxs-lookup"><span data-stu-id="38658-107">Computer Vision</span></span> 

<span data-ttu-id="38658-108">返回图像中找到的视觉内容的相关信息：</span><span class="sxs-lookup"><span data-stu-id="38658-108">Returns information about visual content found in an image:</span></span>

- <span data-ttu-id="38658-109">使用标记、描述和特定于域的模型来识别内容，并使用置信度对其进行标记。</span><span class="sxs-lookup"><span data-stu-id="38658-109">Use tagging, descriptions, and domain-specific models to identify content and label it with confidence.</span></span>
- <span data-ttu-id="38658-110">应用“成人/不雅”设置，以便自动限制成人内容。</span><span class="sxs-lookup"><span data-stu-id="38658-110">Apply adult/racy settings to enable automated restriction of adult content.</span></span>
- <span data-ttu-id="38658-111">识别图片中的图像类型和配色方案。</span><span class="sxs-lookup"><span data-stu-id="38658-111">Identify image types and color schemes in pictures.</span></span>

<span data-ttu-id="38658-112">在浏览器中免费[试用计算机视觉](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/)。</span><span class="sxs-lookup"><span data-stu-id="38658-112">[Try Computer Vision](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/) for free in your browser.</span></span>

<span data-ttu-id="38658-113">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-113">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-vision-computervision
```

<span data-ttu-id="38658-114">[详细了解](/azure/cognitive-services/computer-vision/home)计算机视觉 API 并开始使用[计算机视觉 API Python 快速入门](/azure/cognitive-services/computer-vision/quickstarts/python)。</span><span class="sxs-lookup"><span data-stu-id="38658-114">[Learn more](/azure/cognitive-services/computer-vision/home) about the Computer Vision API and get started with the [Computer Vision API Python quickstart](/azure/cognitive-services/computer-vision/quickstarts/python).</span></span>

### <a name="content-moderator"></a><span data-ttu-id="38658-115">内容审查器</span><span class="sxs-lookup"><span data-stu-id="38658-115">Content Moderator</span></span>

<span data-ttu-id="38658-116">计算机辅助的文本、视频和图像审查，借助人工审阅工具得到增强</span><span class="sxs-lookup"><span data-stu-id="38658-116">Machine-assisted moderation of text, video and images, augmented with human review tools.</span></span>

<span data-ttu-id="38658-117">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-117">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-vision-contentmoderator
```

<span data-ttu-id="38658-118">[详细了解](/azure/cognitive-services/content-moderator/overview)内容审查器服务。</span><span class="sxs-lookup"><span data-stu-id="38658-118">[Learn more](/azure/cognitive-services/content-moderator/overview) about the Content Moderator service.</span></span>

### <a name="custom-vision-service"></a><span data-ttu-id="38658-119">自定义影像服务</span><span class="sxs-lookup"><span data-stu-id="38658-119">Custom Vision Service</span></span>

<span data-ttu-id="38658-120">上传图像，以便针对具体的用例训练和自定义计算机视觉模型。</span><span class="sxs-lookup"><span data-stu-id="38658-120">Upload images to train and customize a computer vision model for your specific use case.</span></span> <span data-ttu-id="38658-121">训练模型以后，即可使用 API 根据模型来标记图像并对结果进行评估，以便改进分类器。</span><span class="sxs-lookup"><span data-stu-id="38658-121">Once the model is trained, you can use the API to tag images using the model and evaluate the results to improve your classifier.</span></span>

<span data-ttu-id="38658-122">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-122">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-vision-customvision
```

<span data-ttu-id="38658-123">[详细了解](/azure/cognitive-services/Custom-Vision-Service/home)自定义视觉服务并开始使用[自定义视觉 Python 教程](/azure/cognitive-services/Custom-Vision-Service/python-tutorial)</span><span class="sxs-lookup"><span data-stu-id="38658-123">[Learn more](/azure/cognitive-services/Custom-Vision-Service/home) about the Custom Vision service and get started with the [Custom Vision Python tutorial](/azure/cognitive-services/Custom-Vision-Service/python-tutorial)</span></span>

### <a name="face-api"></a><span data-ttu-id="38658-124">人脸 API</span><span class="sxs-lookup"><span data-stu-id="38658-124">Face API</span></span>

<span data-ttu-id="38658-125">检测、识别、分析、组织和标记照片中的人脸。</span><span class="sxs-lookup"><span data-stu-id="38658-125">Detect, identify, analyze, organize, and tag faces in photos.</span></span> 

<span data-ttu-id="38658-126">在浏览器中[试用人脸 API](https://azure.microsoft.com/en-us/services/cognitive-services/face/)。</span><span class="sxs-lookup"><span data-stu-id="38658-126">[Try the Face API](https://azure.microsoft.com/en-us/services/cognitive-services/face/) in your browser.</span></span>

<span data-ttu-id="38658-127">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-127">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install cognitive-face
```

<span data-ttu-id="38658-128">[详细了解](/azure/cognitive-services/face/overview)人脸 API 并开始使用[人脸 API Python 快速入门](/azure/cognitive-services/Face/Tutorials/FaceAPIinPythonTutorial)。</span><span class="sxs-lookup"><span data-stu-id="38658-128">[Learn more](/azure/cognitive-services/face/overview) about the Face API and get started with the [Face API Python quickstart](/azure/cognitive-services/Face/Tutorials/FaceAPIinPythonTutorial).</span></span>

## <a name="search-modules"></a><span data-ttu-id="38658-129">搜索模块</span><span class="sxs-lookup"><span data-stu-id="38658-129">Search modules</span></span>

### <a name="web-search"></a><span data-ttu-id="38658-130">Web 搜索</span><span class="sxs-lookup"><span data-stu-id="38658-130">Web search</span></span>

<span data-ttu-id="38658-131">检索必应 Web 搜索 API 索引的 Web 文档，并根据结果类型、新鲜度以及其他特征来缩小结果范围。</span><span class="sxs-lookup"><span data-stu-id="38658-131">Retrieve web documents indexed by the Bing Web Search API and narrow down the results by result type, freshness and more.</span></span> 

<span data-ttu-id="38658-132">在浏览器中[试用 Web 搜索 API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-web-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="38658-132">[Try the Web Search API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-web-search-api/) in your browser.</span></span>

<span data-ttu-id="38658-133">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-133">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-websearch
```

<span data-ttu-id="38658-134">[详细了解](/azure/cognitive-services/bing-web-search/overview)必应 Web 搜索 API 并开始使用 [Web 搜索 API Python 快速入门](/azure/cognitive-services/bing-web-search/quickstarts/python)。</span><span class="sxs-lookup"><span data-stu-id="38658-134">[Learn more](/azure/cognitive-services/bing-web-search/overview) about the Bing Web Search API and get started with the [Web Search API Python quickstart](/azure/cognitive-services/bing-web-search/quickstarts/python).</span></span>

### <a name="image-search"></a><span data-ttu-id="38658-135">图像搜索</span><span class="sxs-lookup"><span data-stu-id="38658-135">Image search</span></span>

<span data-ttu-id="38658-136">搜索图像，在结果中获取缩略图、完整的图像 URL、图像元数据等。</span><span class="sxs-lookup"><span data-stu-id="38658-136">Search for images and get thumbnails, full image URLs, image metadata and more in your results.</span></span>

<span data-ttu-id="38658-137">在浏览器中[试用图像搜索 API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-image-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="38658-137">[Try the Image Search API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-image-search-api/) in your browser.</span></span>

<span data-ttu-id="38658-138">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-138">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-imagesearch
```

<span data-ttu-id="38658-139">[详细了解](/azure/cognitive-services/bing-image-search/overview)必应图像搜索 API 并开始使用[图像搜索 API Python 快速入门](/azure/cognitive-services/bing-image-search/quickstarts/python)。</span><span class="sxs-lookup"><span data-stu-id="38658-139">[Learn more](/azure/cognitive-services/bing-image-search/overview) about the Bing Image Search API and get started with the [Image Search API Python quickstart](/azure/cognitive-services/bing-image-search/quickstarts/python).</span></span>


### <a name="entity-search"></a><span data-ttu-id="38658-140">实体搜索</span><span class="sxs-lookup"><span data-stu-id="38658-140">Entity search</span></span>

<span data-ttu-id="38658-141">针对给定的搜索词或位置，搜索关联性最大的实体（地点、人员或物体）。</span><span class="sxs-lookup"><span data-stu-id="38658-141">Search for the most relevant entity (place, person, or thing) for a given search term or location.</span></span>

<span data-ttu-id="38658-142">在浏览器中[试用实体搜索 API](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="38658-142">[Try the Entity Search API](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) in your browser.</span></span>

<span data-ttu-id="38658-143">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-143">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-entitysearch
```

<span data-ttu-id="38658-144">[详细了解](/azure/cognitive-services/bing-entities-search/search-the-web)必应实体搜索 API 并开始使用[实体搜索 API Python 快速入门](/azure/cognitive-services/bing-entities-search/quickstarts/python)。</span><span class="sxs-lookup"><span data-stu-id="38658-144">[Learn more](/azure/cognitive-services/bing-entities-search/search-the-web) about the Bing Entity Search API and get started with the [Entity Search API Python quickstart](/azure/cognitive-services/bing-entities-search/quickstarts/python).</span></span>

### <a name="custom-search"></a><span data-ttu-id="38658-145">自定义搜索</span><span class="sxs-lookup"><span data-stu-id="38658-145">Custom search</span></span>

<span data-ttu-id="38658-146">构建符合特定搜索域要求的自定义 Web 搜索。</span><span class="sxs-lookup"><span data-stu-id="38658-146">Build and a custom web search that meets your specific search domain.</span></span>

<span data-ttu-id="38658-147">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-147">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-customsearch
```

<span data-ttu-id="38658-148">[详细了解](/azure/cognitive-services/bing-custom-search/)必应自定义搜索服务并开始使用[自定义搜索 API Python 快速入门](/azure/cognitive-services/bing-custom-search/call-endpoint-python)从 Python 查询自定义搜索。</span><span class="sxs-lookup"><span data-stu-id="38658-148">[Learn more](/azure/cognitive-services/bing-custom-search/) about the Bing Custom Search service and get started with querying your custom search from Python with the [Custom Search API Python quickstart](/azure/cognitive-services/bing-custom-search/call-endpoint-python).</span></span>

### <a name="video-search"></a><span data-ttu-id="38658-149">视频搜索</span><span class="sxs-lookup"><span data-stu-id="38658-149">Video search</span></span>

<span data-ttu-id="38658-150">在 Web 中查找视频，获取包含创建者、编码、长度和观看次数等元数据在内的结果。</span><span class="sxs-lookup"><span data-stu-id="38658-150">Find videos across the web and get results with creator, encoding, length, and view count metadata.</span></span>

<span data-ttu-id="38658-151">在浏览器中[试用视频搜索 API](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="38658-151">[Try the Video Search API](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) in your browser.</span></span>

<span data-ttu-id="38658-152">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-152">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-videosearch
```

<span data-ttu-id="38658-153">[详细了解](/azure/cognitive-services/bing-video-search/search-the-web)必应视频搜索服务并开始使用[视频搜索 API Python 快速入门](/azure/cognitive-services/bing-video-search/python)。</span><span class="sxs-lookup"><span data-stu-id="38658-153">[Learn more](/azure/cognitive-services/bing-video-search/search-the-web) about the Bing Video Search service and get started with the [Video Search API Python quickstart](/azure/cognitive-services/bing-video-search/python).</span></span>


### <a name="news-search"></a><span data-ttu-id="38658-154">新闻搜索</span><span class="sxs-lookup"><span data-stu-id="38658-154">News search</span></span>

<span data-ttu-id="38658-155">在 Web 中搜索新闻文章，并对文章、相关新闻、图像和提供者信息等元数据进行处理。</span><span class="sxs-lookup"><span data-stu-id="38658-155">Search the web for news articles and work with article, related news, images, and provider info metadata.</span></span>

<span data-ttu-id="38658-156">在浏览器中[试用新闻搜索 API](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="38658-156">[Try the News Search API](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/) in your browser.</span></span>

<span data-ttu-id="38658-157">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-157">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-newssearch
```

<span data-ttu-id="38658-158">[详细了解](/azure/cognitive-services/bing-news-search/search-the-web)必应新闻搜索服务并开始使用[新闻搜索 API Python 快速入门](/azure/cognitive-services/bing-news-search/python)。</span><span class="sxs-lookup"><span data-stu-id="38658-158">[Learn more](/azure/cognitive-services/bing-news-search/search-the-web) about the Bing News Search service and get started with the [News Search API Python quickstart](/azure/cognitive-services/bing-news-search/python).</span></span>


## <a name="language-modules"></a><span data-ttu-id="38658-159">语言模块</span><span class="sxs-lookup"><span data-stu-id="38658-159">Language modules</span></span>

### <a name="text-analytics"></a><span data-ttu-id="38658-160">文本分析</span><span class="sxs-lookup"><span data-stu-id="38658-160">Text Analytics</span></span> 

<span data-ttu-id="38658-161">文本分析 API 是一种基于云的服务，提供对原始文本的自然语言处理。</span><span class="sxs-lookup"><span data-stu-id="38658-161">The Text Analytics API is a cloud-based service that provides  natural language processing over raw text.</span></span> <span data-ttu-id="38658-162">此 API 包括三项主要功能：</span><span class="sxs-lookup"><span data-stu-id="38658-162">The API includes three main functions:</span></span>

- <span data-ttu-id="38658-163">情绪分析</span><span class="sxs-lookup"><span data-stu-id="38658-163">Sentiment analysis</span></span>
- <span data-ttu-id="38658-164">关键短语提取</span><span class="sxs-lookup"><span data-stu-id="38658-164">Key phrase extraction</span></span>
- <span data-ttu-id="38658-165">语言检测</span><span class="sxs-lookup"><span data-stu-id="38658-165">Language detection</span></span>

<span data-ttu-id="38658-166">在浏览器中[试用文本分析 API](https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/)。</span><span class="sxs-lookup"><span data-stu-id="38658-166">[Try the Text Analytics API](https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/) in your browser.</span></span>

<span data-ttu-id="38658-167">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-167">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-language-textanalytics
```

<span data-ttu-id="38658-168">[详细了解](/azure/cognitive-services/text-analytics/overview)文本分析 API 并开始使用[文本分析 API Python 快速入门](/azure/cognitive-services/text-analytics/quickstarts/python)。</span><span class="sxs-lookup"><span data-stu-id="38658-168">[Learn more](/azure/cognitive-services/text-analytics/overview) about the Text Analytics API and get started with the [Text Analytics API Python quickstart](/azure/cognitive-services/text-analytics/quickstarts/python).</span></span>


### <a name="spell-check"></a><span data-ttu-id="38658-169">拼写检查</span><span class="sxs-lookup"><span data-stu-id="38658-169">Spell Check</span></span>

<span data-ttu-id="38658-170">使用必应拼写检查 API 进行上下文语法和拼写检查。</span><span class="sxs-lookup"><span data-stu-id="38658-170">Perform contextual grammar and spell checking with the Bing Spell Check API.</span></span>

<span data-ttu-id="38658-171">在浏览器中[试用拼写检查 API](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/)。</span><span class="sxs-lookup"><span data-stu-id="38658-171">[Try the Spell Check API](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/) in your browser.</span></span>

<span data-ttu-id="38658-172">获取包含 [pip](https://pip.pypa.io/en/stable/quickstart/) 的 Python 模块：</span><span class="sxs-lookup"><span data-stu-id="38658-172">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-language-spellcheck
```

<span data-ttu-id="38658-173">[详细了解](/azure/cognitive-services/bing-spell-check/proof-text)拼写检查 API 并开始使用[拼写检查 API Python 快速入门](/azure/cognitive-services/bing-spell-check/quickstarts/python)。</span><span class="sxs-lookup"><span data-stu-id="38658-173">[Learn more](/azure/cognitive-services/bing-spell-check/proof-text) about the Spell Check API and get started with the [Spell Check API Python quickstart](/azure/cognitive-services/bing-spell-check/quickstarts/python).</span></span>
