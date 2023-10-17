### Context
Multimodal question-answering (QA) systems are gaining prominence as they can comprehend both text and visual information. Multimodal content is prevalent in today's digital landscape, often found in documents such as PDFs that combine text and images. Traditional OCR models cannot generate descriptions or context for images. While OCR can recognize and extract text within images, it does not provide meaningful descriptions of the visual elements. This is a critical limitation when attempting to perform QA on images or when seeking to provide image context for users. Moreover, text-to-text models alone would not be sufficient to perform the specific task of extracting image descriptions from PDF documents. A combination of text-to-text models and image-to-text models is essential for processing PDF documents with mixed text and visual content.

### Pattern
The pattern involves combining BLIP-2 and Claude in a multimodal QA system. BLIP-2 excels in image understanding, while Claude is a powerful text-based language model available via Bedrock API. The two models work synergistically to provide comprehensive answers to user queries, which can involve a combination of text and images. The proposed solution is to initiate with a PDF document, utilizing Blip-2 to process the images, generate image descriptions, and comprehend their visual content. Claude processes the textual content within the document. Users can then pose questions related to both the textual and visual aspects of the content. The system analyzes these queries, utilizes BLIP-2 for image description and Claude for text understanding, thereby facilitating comprehensive and intuitive question-answering. This approach enhances content extraction and QA on multimodal documents, making it valuable in fields like education, research, content curation, and more.


### Challenges

- Integrating BLIP-2 and Claude to handle both text and images in PDF documents.
- Maintaining context between textual and visual elements to ensure meaningful answers in multimodal QA.

### Proposal
To the above challenges, this notebook proposes the following strategy

#### Deploy BLIP-2 to a SageMaker endpoint
You can host an LLM on SageMaker using the Large Model Inference (LMI) container that is optimized for hosting large models using DJLServing. 

- Retrieve the Docker image of DJLServing
- Package the inference script and configuration files as a model.tar.gz file
- Upload the model.tar.gz file to Amazon S3 bucket
- Create the model, the configuration for the endpoint, and the endpoint

More details can be found in this blog: https://aws.amazon.com/it/blogs/machine-learning/build-a-generative-ai-based-content-moderation-solution-on-amazon-sagemaker-jumpstart/

#### Load, split, and invoke the endpoint
When the endpoint is deployed, you are ready to extract the text and image from a PDF, and invoke the endpoint to generate the image description and comprehend its visual content.

- Load a pdf from local disk
- Extract text and image
- Invoke the BLIP-2 endpoint

#### Invoke the Bedrock API for QA
After extracting image description, you can invoke the Bedrock API with a predifined prompt template to produce organized QA responses based on user queries and the extracted content.

- Create prompt templates
- Invoke the Bedrock API 
