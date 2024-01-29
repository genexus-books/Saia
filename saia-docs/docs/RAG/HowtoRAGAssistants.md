---
sidebar_label: 'How to create and test a RAG Assistant'
sidebar_position: 2
---
# How to create and test a RAG Assistant

Here is a step-by-step guide on how to use RAG Assistants, upload a document, and start chatting with the assistant from the playground.

## Step 1: Create New

First, enter the GeneXus Enterprise AI backoffice. On the left side of the screen, you can find the backoffice menu. In this menu, click 
on RAG Assistants.

Next, in the Project Dynamic Combo Box, select the project you want to work with (in this case, the Default one is used).

![image](https://github.com/genexus-books/Saia/blob/fc9c844045233f79c3149780365a6100663c642b/saia-docs/assets/images/RAGAssistantsSection7.png?raw=true)

By default, when opening the RAG Assistants section, a RAG Assistant named Default is displayed.

Create a New RAG Assistant by clicking on the CREATE NEW button.

![image](https://github.com/genexus-books/Saia/blob/fc9c844045233f79c3149780365a6100663c642b/saia-docs/assets/images/RAGAssistantsSection8.png?raw=true)

After you click on the CREATE NEW button, a window will appear to enter a Name and Description:

![image](https://github.com/genexus-books/Saia/blob/37ab69ed86934b9056f4ec4c1c398fa1af28181f/saia-docs/assets/images/RAGAssistantsSection9.png?raw=true)

Next, click on the CONFIRM button.

## Step 2: Set RAG Assistants

Once the RAG Assistant is created, you return to the main page that provides the UPDATE option. This option allows you to customize the RAG 
Assistant to your needs.

![image](https://github.com/genexus-books/Saia/blob/fc9c844045233f79c3149780365a6100663c642b/saia-docs/assets/images/RAGAssistantsSection10.png?raw=true)

For more information, see: [RAG Assistants section](https://docs.saia.ai/RAG/RAGAssistantsSection)

## Step 3: Upload documents

To add documents, go to the RAG Assistants home page and click on ADD DOCUMENTS.

![image](https://github.com/genexus-books/Saia/blob/074a13e6e94f08afb5fb100ba8ac4bb8c3ea891f/saia-docs/assets/images/RAGAssistantsSection11.png?raw=true)

This will open a window similar to the one below. Click on the Add files button to upload files of type .txt, .pdf, .docx, .pptx, .xlsx, 
.odt, .odp, .ods, .xlsx, .epub, .json, .jsonl, and .csv.

In addition, it is possible to add metadata by clicking on New row.

![image](https://github.com/genexus-books/Saia/blob/7a57083c558e920f6c9167dcbaf8794846da6e15/saia-docs/assets/images/RAGAssistantsSection12.png?raw=true)

The format is a key/value configuration for each row. For example:

Name: 'year'
Value: 2003

Later, during the retrieval process, it is possible to filter according to specific metadata.

Finally, click on START UPLOAD to upload the document.

## Step 4: Customize Index
This section allows you to configure indexes, which are essential for structuring unstructured documents so that LLMs can optimally 
interact with them.

To configure Indexes to your needs, go to the RAG Assistants main page and click on INDEX.

![image](https://github.com/genexus-books/Saia/blob/fc9c844045233f79c3149780365a6100663c642b/saia-docs/assets/images/RAGAssistantsSection13.png?raw=true)

If you want to configure the default values taken by Index, click on UPDATE:

![image](https://github.com/genexus-books/Saia/blob/fc9c844045233f79c3149780365a6100663c642b/saia-docs/assets/images/RAGAssistantsSection14.png?raw=true)

To customize the Chunk Size, Chunk Overlap, and Extra Chunks values, take into account that these parameters are critical to tailor LLM 
processing to your specific needs.

The default settings include the maximum character limit per chunk and the number of overlapping characters between adjacent chunks. 
Modifying these values allows you to adjust the document partitioning process according to your preferences and application
requirements.

**Note:** Keep in mind that if you make changes to the chunks configuration, you must re-ingest the documents so that GeneXus Enterprise AI 
takes the new configuration into account. To do so, delete the existing documents and upload a new one. 

In addition, you can explore additional partitioning strategies using the Extra Chunks element. This component gives you the flexibility to
add additional chunks and test different configurations to determine the most effective partitioning strategy in your specific case.

For example, if you wish to add two additional chunks, you can populate the Extra Chunks parameter as shown below:

```
[
  {
      "chunkSize":500,
      "chunkOverlap":80
  },
  {
      "chunkSize":3000,
      "chunkOverlap":100
  }
]
```

In this case, each document will be chunked three times and ingested separately. Later on, when querying you can change the default 
namespace by appending the extra chunkSize value.

To save the changes, click on CONFIRM.

## Step 5: View documents

To view the documents and the details of each one, go to the RAG Assistants home page and click on VIEW DOCUMENTS.

![image](https://github.com/genexus-books/Saia/blob/fc9c844045233f79c3149780365a6100663c642b/saia-docs/assets/images/RAGAssistantsSection15.png?raw=true)

In the specific case of MyRagAssistant, only one document called IRT is available. Here, you can click on the document name (IRT) to access 
its details or, if you prefer, delete the document.

![image](https://github.com/genexus-books/Saia/blob/fc9c844045233f79c3149780365a6100663c642b/saia-docs/assets/images/RAGAssistantsSection16.png?raw=true)

By clicking on the document name, in this case IRT, you will find two subsections: General and Metadata

In the General section, you can view the following information:

![image](https://github.com/genexus-books/Saia/blob/fc9c844045233f79c3149780365a6100663c642b/saia-docs/assets/images/RAGAssistantsSection17.png?raw=true)

By clicking on Document Url, you can view/download the document.

On the other hand, in the Metadata section, you can view the following information:

Metadata Name: IRT
Metadata Value: 2015

## Step 6: Playground

Finally, you can test your RAG Assistant by clicking on Playground on the left side menu of the backoffice window:

![image](https://github.com/genexus-books/Saia/blob/fc9c844045233f79c3149780365a6100663c642b/saia-docs/assets/images/RAGAssistantsSection18.png?raw=true)

In the following image, you can see it running:

![image](https://github.com/genexus-books/Saia/blob/fc9c844045233f79c3149780365a6100663c642b/saia-docs/assets/images/RAGAssistantsSection19.png?raw=true)







