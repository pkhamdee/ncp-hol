# Flowise Chatflows

chatflow เหล่านี้สามารถดาวน์โหลดและ import เข้าสู่ Flowise ได้

-   Chatbot Flow

```
{
  "nodes": [
    {
      "id": "chatOpenAICustom_0",
      "position": {
        "x": -99.59846636021855,
        "y": -90.74324159189774
      },
      "type": "customNode",
      "data": {
        "id": "chatOpenAICustom_0",
        "label": "ChatOpenAI Custom",
        "version": 4,
        "name": "chatOpenAICustom",
        "type": "ChatOpenAI-Custom",
        "baseClasses": [
          "ChatOpenAI-Custom",
          "BaseChatModel",
          "BaseLanguageModel",
          "Runnable"
        ],
        "category": "Chat Models",
        "description": "Custom/FineTuned model using OpenAI Chat compatible API",
        "inputParams": [
          {
            "label": "Connect Credential",
            "name": "credential",
            "type": "credential",
            "credentialNames": [
              "openAIApi"
            ],
            "optional": true,
            "id": "chatOpenAICustom_0-input-credential-credential"
          },
          {
            "label": "Model Name",
            "name": "modelName",
            "type": "string",
            "placeholder": "ft:gpt-3.5-turbo:my-org:custom_suffix:id",
            "id": "chatOpenAICustom_0-input-modelName-string"
          },
          {
            "label": "Temperature",
            "name": "temperature",
            "type": "number",
            "step": 0.1,
            "default": 0.9,
            "optional": true,
            "id": "chatOpenAICustom_0-input-temperature-number"
          },
          {
            "label": "Streaming",
            "name": "streaming",
            "type": "boolean",
            "default": true,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-streaming-boolean"
          },
          {
            "label": "Max Tokens",
            "name": "maxTokens",
            "type": "number",
            "step": 1,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-maxTokens-number"
          },
          {
            "label": "Top Probability",
            "name": "topP",
            "type": "number",
            "step": 0.1,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-topP-number"
          },
          {
            "label": "Frequency Penalty",
            "name": "frequencyPenalty",
            "type": "number",
            "step": 0.1,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-frequencyPenalty-number"
          },
          {
            "label": "Presence Penalty",
            "name": "presencePenalty",
            "type": "number",
            "step": 0.1,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-presencePenalty-number"
          },
          {
            "label": "Timeout",
            "name": "timeout",
            "type": "number",
            "step": 1,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-timeout-number"
          },
          {
            "label": "BasePath",
            "name": "basepath",
            "type": "string",
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-basepath-string"
          },
          {
            "label": "BaseOptions",
            "name": "baseOptions",
            "type": "json",
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-baseOptions-json"
          }
        ],
        "inputAnchors": [
          {
            "label": "Cache",
            "name": "cache",
            "type": "BaseCache",
            "optional": true,
            "id": "chatOpenAICustom_0-input-cache-BaseCache"
          }
        ],
        "inputs": {
          "cache": "",
          "modelName": "llama3-1-8b",
          "temperature": 0.9,
          "streaming": true,
          "maxTokens": "4096",
          "topP": "",
          "frequencyPenalty": "",
          "presencePenalty": "",
          "timeout": "",
          "basepath": "https://nai.tmelab.net/api/v1",
          "baseOptions": ""
        },
        "outputAnchors": [
          {
            "id": "chatOpenAICustom_0-output-chatOpenAICustom-ChatOpenAI-Custom|BaseChatModel|BaseLanguageModel|Runnable",
            "name": "chatOpenAICustom",
            "label": "ChatOpenAI-Custom",
            "description": "Custom/FineTuned model using OpenAI Chat compatible API",
            "type": "ChatOpenAI-Custom | BaseChatModel | BaseLanguageModel | Runnable"
          }
        ],
        "outputs": {},
        "selected": false
      },
      "width": 300,
      "height": 577,
      "selected": false,
      "positionAbsolute": {
        "x": -99.59846636021855,
        "y": -90.74324159189774
      },
      "dragging": false
    },
    {
      "id": "conversationChain_0",
      "position": {
        "x": 479.8018614228041,
        "y": 136.81254183068958
      },
      "type": "customNode",
      "data": {
        "id": "conversationChain_0",
        "label": "Conversation Chain",
        "version": 3,
        "name": "conversationChain",
        "type": "ConversationChain",
        "baseClasses": [
          "ConversationChain",
          "LLMChain",
          "BaseChain",
          "Runnable"
        ],
        "category": "Chains",
        "description": "Chat models specific conversational chain with memory",
        "inputParams": [
          {
            "label": "System Message",
            "name": "systemMessagePrompt",
            "type": "string",
            "rows": 4,
            "description": "If Chat Prompt Template is provided, this will be ignored",
            "additionalParams": true,
            "optional": true,
            "default": "The following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.",
            "placeholder": "The following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.",
            "id": "conversationChain_0-input-systemMessagePrompt-string"
          }
        ],
        "inputAnchors": [
          {
            "label": "Chat Model",
            "name": "model",
            "type": "BaseChatModel",
            "id": "conversationChain_0-input-model-BaseChatModel"
          },
          {
            "label": "Memory",
            "name": "memory",
            "type": "BaseMemory",
            "id": "conversationChain_0-input-memory-BaseMemory"
          },
          {
            "label": "Chat Prompt Template",
            "name": "chatPromptTemplate",
            "type": "ChatPromptTemplate",
            "description": "Override existing prompt with Chat Prompt Template. Human Message must includes {input} variable",
            "optional": true,
            "id": "conversationChain_0-input-chatPromptTemplate-ChatPromptTemplate"
          },
          {
            "label": "Input Moderation",
            "description": "Detect text that could generate harmful output and prevent it from being sent to the language model",
            "name": "inputModeration",
            "type": "Moderation",
            "optional": true,
            "list": true,
            "id": "conversationChain_0-input-inputModeration-Moderation"
          }
        ],
        "inputs": {
          "model": "{{chatOpenAICustom_0.data.instance}}",
          "memory": "{{bufferWindowMemory_0.data.instance}}",
          "chatPromptTemplate": "",
          "inputModeration": "",
          "systemMessagePrompt": "The following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know."
        },
        "outputAnchors": [
          {
            "id": "conversationChain_0-output-conversationChain-ConversationChain|LLMChain|BaseChain|Runnable",
            "name": "conversationChain",
            "label": "ConversationChain",
            "description": "Chat models specific conversational chain with memory",
            "type": "ConversationChain | LLMChain | BaseChain | Runnable"
          }
        ],
        "outputs": {},
        "selected": false
      },
      "width": 300,
      "height": 435,
      "selected": false,
      "positionAbsolute": {
        "x": 479.8018614228041,
        "y": 136.81254183068958
      },
      "dragging": false
    },
    {
      "id": "bufferWindowMemory_0",
      "position": {
        "x": -88.84450930074689,
        "y": 495.22548476454295
      },
      "type": "customNode",
      "data": {
        "id": "bufferWindowMemory_0",
        "label": "Buffer Window Memory",
        "version": 2,
        "name": "bufferWindowMemory",
        "type": "BufferWindowMemory",
        "baseClasses": [
          "BufferWindowMemory",
          "BaseChatMemory",
          "BaseMemory"
        ],
        "category": "Memory",
        "description": "Uses a window of size k to surface the last k back-and-forth to use as memory",
        "inputParams": [
          {
            "label": "Size",
            "name": "k",
            "type": "number",
            "default": "4",
            "description": "Window of size k to surface the last k back-and-forth to use as memory.",
            "id": "bufferWindowMemory_0-input-k-number"
          },
          {
            "label": "Session Id",
            "name": "sessionId",
            "type": "string",
            "description": "If not specified, a random id will be used. Learn <a target=\"_blank\" href=\"https://docs.flowiseai.com/memory#ui-and-embedded-chat\">more</a>",
            "default": "",
            "optional": true,
            "additionalParams": true,
            "id": "bufferWindowMemory_0-input-sessionId-string"
          },
          {
            "label": "Memory Key",
            "name": "memoryKey",
            "type": "string",
            "default": "chat_history",
            "additionalParams": true,
            "id": "bufferWindowMemory_0-input-memoryKey-string"
          }
        ],
        "inputAnchors": [],
        "inputs": {
          "k": "4",
          "sessionId": "",
          "memoryKey": "chat_history"
        },
        "outputAnchors": [
          {
            "id": "bufferWindowMemory_0-output-bufferWindowMemory-BufferWindowMemory|BaseChatMemory|BaseMemory",
            "name": "bufferWindowMemory",
            "label": "BufferWindowMemory",
            "description": "Uses a window of size k to surface the last k back-and-forth to use as memory",
            "type": "BufferWindowMemory | BaseChatMemory | BaseMemory"
          }
        ],
        "outputs": {},
        "selected": false
      },
      "width": 300,
      "height": 331,
      "selected": false,
      "positionAbsolute": {
        "x": -88.84450930074689,
        "y": 495.22548476454295
      },
      "dragging": false
    }
  ],
  "edges": [
    {
      "source": "chatOpenAICustom_0",
      "sourceHandle": "chatOpenAICustom_0-output-chatOpenAICustom-ChatOpenAI-Custom|BaseChatModel|BaseLanguageModel|Runnable",
      "target": "conversationChain_0",
      "targetHandle": "conversationChain_0-input-model-BaseChatModel",
      "type": "buttonedge",
      "id": "chatOpenAICustom_0-chatOpenAICustom_0-output-chatOpenAICustom-ChatOpenAI-Custom|BaseChatModel|BaseLanguageModel|Runnable-conversationChain_0-conversationChain_0-input-model-BaseChatModel"
    },
    {
      "source": "bufferWindowMemory_0",
      "sourceHandle": "bufferWindowMemory_0-output-bufferWindowMemory-BufferWindowMemory|BaseChatMemory|BaseMemory",
      "target": "conversationChain_0",
      "targetHandle": "conversationChain_0-input-memory-BaseMemory",
      "type": "buttonedge",
      "id": "bufferWindowMemory_0-bufferWindowMemory_0-output-bufferWindowMemory-BufferWindowMemory|BaseChatMemory|BaseMemory-conversationChain_0-conversationChain_0-input-memory-BaseMemory"
    }
  ]
}
```

-   Chatbot Flow with RAG

```
{
  "nodes": [
    {
      "id": "chatOpenAICustom_0",
      "position": {
        "x": -492.16776458289974,
        "y": -120.08116067615506
      },
      "type": "customNode",
      "data": {
        "id": "chatOpenAICustom_0",
        "label": "ChatOpenAI Custom",
        "version": 4,
        "name": "chatOpenAICustom",
        "type": "ChatOpenAI-Custom",
        "baseClasses": [
          "ChatOpenAI-Custom",
          "BaseChatModel",
          "BaseLanguageModel",
          "Runnable"
        ],
        "category": "Chat Models",
        "description": "Custom/FineTuned model using OpenAI Chat compatible API",
        "inputParams": [
          {
            "label": "Connect Credential",
            "name": "credential",
            "type": "credential",
            "credentialNames": [
              "openAIApi"
            ],
            "optional": true,
            "id": "chatOpenAICustom_0-input-credential-credential"
          },
          {
            "label": "Model Name",
            "name": "modelName",
            "type": "string",
            "placeholder": "ft:gpt-3.5-turbo:my-org:custom_suffix:id",
            "id": "chatOpenAICustom_0-input-modelName-string"
          },
          {
            "label": "Temperature",
            "name": "temperature",
            "type": "number",
            "step": 0.1,
            "default": 0.9,
            "optional": true,
            "id": "chatOpenAICustom_0-input-temperature-number"
          },
          {
            "label": "Streaming",
            "name": "streaming",
            "type": "boolean",
            "default": true,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-streaming-boolean"
          },
          {
            "label": "Max Tokens",
            "name": "maxTokens",
            "type": "number",
            "step": 1,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-maxTokens-number"
          },
          {
            "label": "Top Probability",
            "name": "topP",
            "type": "number",
            "step": 0.1,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-topP-number"
          },
          {
            "label": "Frequency Penalty",
            "name": "frequencyPenalty",
            "type": "number",
            "step": 0.1,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-frequencyPenalty-number"
          },
          {
            "label": "Presence Penalty",
            "name": "presencePenalty",
            "type": "number",
            "step": 0.1,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-presencePenalty-number"
          },
          {
            "label": "Timeout",
            "name": "timeout",
            "type": "number",
            "step": 1,
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-timeout-number"
          },
          {
            "label": "BasePath",
            "name": "basepath",
            "type": "string",
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-basepath-string"
          },
          {
            "label": "BaseOptions",
            "name": "baseOptions",
            "type": "json",
            "optional": true,
            "additionalParams": true,
            "id": "chatOpenAICustom_0-input-baseOptions-json"
          }
        ],
        "inputAnchors": [
          {
            "label": "Cache",
            "name": "cache",
            "type": "BaseCache",
            "optional": true,
            "id": "chatOpenAICustom_0-input-cache-BaseCache"
          }
        ],
        "inputs": {
          "cache": "",
          "modelName": "llama3-1-8b",
          "temperature": 0.9,
          "streaming": true,
          "maxTokens": "4096",
          "topP": "",
          "frequencyPenalty": "",
          "presencePenalty": "",
          "timeout": "",
          "basepath": "https://nai.tmelab.net/api/v1",
          "baseOptions": ""
        },
        "outputAnchors": [
          {
            "id": "chatOpenAICustom_0-output-chatOpenAICustom-ChatOpenAI-Custom|BaseChatModel|BaseLanguageModel|Runnable",
            "name": "chatOpenAICustom",
            "label": "ChatOpenAI-Custom",
            "description": "Custom/FineTuned model using OpenAI Chat compatible API",
            "type": "ChatOpenAI-Custom | BaseChatModel | BaseLanguageModel | Runnable"
          }
        ],
        "outputs": {},
        "selected": false
      },
      "width": 300,
      "height": 577,
      "selected": false,
      "positionAbsolute": {
        "x": -492.16776458289974,
        "y": -120.08116067615506
      },
      "dragging": false
    },
    {
      "id": "bufferWindowMemory_0",
      "position": {
        "x": -155.90261006476362,
        "y": -85.94472185693522
      },
      "type": "customNode",
      "data": {
        "id": "bufferWindowMemory_0",
        "label": "Buffer Window Memory",
        "version": 2,
        "name": "bufferWindowMemory",
        "type": "BufferWindowMemory",
        "baseClasses": [
          "BufferWindowMemory",
          "BaseChatMemory",
          "BaseMemory"
        ],
        "category": "Memory",
        "description": "Uses a window of size k to surface the last k back-and-forth to use as memory",
        "inputParams": [
          {
            "label": "Size",
            "name": "k",
            "type": "number",
            "default": "4",
            "description": "Window of size k to surface the last k back-and-forth to use as memory.",
            "id": "bufferWindowMemory_0-input-k-number"
          },
          {
            "label": "Session Id",
            "name": "sessionId",
            "type": "string",
            "description": "If not specified, a random id will be used. Learn <a target=\"_blank\" href=\"https://docs.flowiseai.com/memory#ui-and-embedded-chat\">more</a>",
            "default": "",
            "optional": true,
            "additionalParams": true,
            "id": "bufferWindowMemory_0-input-sessionId-string"
          },
          {
            "label": "Memory Key",
            "name": "memoryKey",
            "type": "string",
            "default": "chat_history",
            "additionalParams": true,
            "id": "bufferWindowMemory_0-input-memoryKey-string"
          }
        ],
        "inputAnchors": [],
        "inputs": {
          "k": "4",
          "sessionId": "",
          "memoryKey": "chat_history"
        },
        "outputAnchors": [
          {
            "id": "bufferWindowMemory_0-output-bufferWindowMemory-BufferWindowMemory|BaseChatMemory|BaseMemory",
            "name": "bufferWindowMemory",
            "label": "BufferWindowMemory",
            "description": "Uses a window of size k to surface the last k back-and-forth to use as memory",
            "type": "BufferWindowMemory | BaseChatMemory | BaseMemory"
          }
        ],
        "outputs": {},
        "selected": false
      },
      "width": 300,
      "height": 331,
      "selected": false,
      "positionAbsolute": {
        "x": -155.90261006476362,
        "y": -85.94472185693522
      },
      "dragging": false
    },
    {
      "id": "conversationalRetrievalQAChain_0",
      "position": {
        "x": 460.24746140015617,
        "y": 134.43481267818106
      },
      "type": "customNode",
      "data": {
        "id": "conversationalRetrievalQAChain_0",
        "label": "Conversational Retrieval QA Chain",
        "version": 3,
        "name": "conversationalRetrievalQAChain",
        "type": "ConversationalRetrievalQAChain",
        "baseClasses": [
          "ConversationalRetrievalQAChain",
          "BaseChain",
          "Runnable"
        ],
        "category": "Chains",
        "description": "Document QA - built on RetrievalQAChain to provide a chat history component",
        "inputParams": [
          {
            "label": "Return Source Documents",
            "name": "returnSourceDocuments",
            "type": "boolean",
            "optional": true,
            "id": "conversationalRetrievalQAChain_0-input-returnSourceDocuments-boolean"
          },
          {
            "label": "Rephrase Prompt",
            "name": "rephrasePrompt",
            "type": "string",
            "description": "Using previous chat history, rephrase question into a standalone question",
            "warning": "Prompt must include input variables: {chat_history} and {question}",
            "rows": 4,
            "additionalParams": true,
            "optional": true,
            "default": "Given the following conversation and a follow up question, rephrase the follow up question to be a standalone question.\n\nChat History:\n{chat_history}\nFollow Up Input: {question}\nStandalone Question:",
            "id": "conversationalRetrievalQAChain_0-input-rephrasePrompt-string"
          },
          {
            "label": "Response Prompt",
            "name": "responsePrompt",
            "type": "string",
            "description": "Taking the rephrased question, search for answer from the provided context",
            "warning": "Prompt must include input variable: {context}",
            "rows": 4,
            "additionalParams": true,
            "optional": true,
            "default": "I want you to act as a document that I am having a conversation with. Your name is \"AI Assistant\". Using the provided context, answer the user's question to the best of your ability using the resources provided.\nIf there is nothing in the context relevant to the question at hand, just say \"Hmm, I'm not sure\" and stop after that. Refuse to answer any question not about the info. Never break character.\n------------\n{context}\n------------\nREMEMBER: If there is no relevant information within the context, just say \"Hmm, I'm not sure\". Don't try to make up an answer. Never break character.",
            "id": "conversationalRetrievalQAChain_0-input-responsePrompt-string"
          }
        ],
        "inputAnchors": [
          {
            "label": "Chat Model",
            "name": "model",
            "type": "BaseChatModel",
            "id": "conversationalRetrievalQAChain_0-input-model-BaseChatModel"
          },
          {
            "label": "Vector Store Retriever",
            "name": "vectorStoreRetriever",
            "type": "BaseRetriever",
            "id": "conversationalRetrievalQAChain_0-input-vectorStoreRetriever-BaseRetriever"
          },
          {
            "label": "Memory",
            "name": "memory",
            "type": "BaseMemory",
            "optional": true,
            "description": "If left empty, a default BufferMemory will be used",
            "id": "conversationalRetrievalQAChain_0-input-memory-BaseMemory"
          },
          {
            "label": "Input Moderation",
            "description": "Detect text that could generate harmful output and prevent it from being sent to the language model",
            "name": "inputModeration",
            "type": "Moderation",
            "optional": true,
            "list": true,
            "id": "conversationalRetrievalQAChain_0-input-inputModeration-Moderation"
          }
        ],
        "inputs": {
          "model": "{{chatOpenAICustom_0.data.instance}}",
          "vectorStoreRetriever": "{{documentStoreVS_0.data.instance}}",
          "memory": "{{bufferWindowMemory_0.data.instance}}",
          "returnSourceDocuments": "",
          "rephrasePrompt": "Given the following conversation and a follow up question, rephrase the follow up question to be a standalone question.\n\nChat History:\n{chat_history}\nFollow Up Input: {question}\nStandalone Question:",
          "responsePrompt": "I want you to act as a document that I am having a conversation with. Your name is \"AI Assistant\". Using the provided context, answer the user's question to the best of your ability using the resources provided.\nIf there is nothing in the context relevant to the question at hand, just say \"Hmm, I'm not sure\" and stop after that. Refuse to answer any question not about the info. Never break character.\n------------\n{context}\n------------\nREMEMBER: If there is no relevant information within the context, just say \"Hmm, I'm not sure\". Don't try to make up an answer. Never break character.",
          "inputModeration": ""
        },
        "outputAnchors": [
          {
            "id": "conversationalRetrievalQAChain_0-output-conversationalRetrievalQAChain-ConversationalRetrievalQAChain|BaseChain|Runnable",
            "name": "conversationalRetrievalQAChain",
            "label": "ConversationalRetrievalQAChain",
            "description": "Document QA - built on RetrievalQAChain to provide a chat history component",
            "type": "ConversationalRetrievalQAChain | BaseChain | Runnable"
          }
        ],
        "outputs": {},
        "selected": false
      },
      "width": 300,
      "height": 532,
      "positionAbsolute": {
        "x": 460.24746140015617,
        "y": 134.43481267818106
      },
      "selected": false
    },
    {
      "id": "documentStoreVS_0",
      "position": {
        "x": -122.31978898723906,
        "y": 451.56374754134333
      },
      "type": "customNode",
      "data": {
        "id": "documentStoreVS_0",
        "label": "Document Store (Vector)",
        "version": 1,
        "name": "documentStoreVS",
        "type": "DocumentStoreVS",
        "baseClasses": [
          "DocumentStoreVS"
        ],
        "category": "Vector Stores",
        "description": "Search and retrieve documents from Document Store",
        "inputParams": [
          {
            "label": "Select Store",
            "name": "selectedStore",
            "type": "asyncOptions",
            "loadMethod": "listStores",
            "id": "documentStoreVS_0-input-selectedStore-asyncOptions"
          }
        ],
        "inputAnchors": [],
        "inputs": {
          "selectedStore": "bca2f479-a538-4fbe-b783-1797af905da7"
        },
        "outputAnchors": [
          {
            "name": "output",
            "label": "Output",
            "type": "options",
            "description": "",
            "options": [
              {
                "id": "documentStoreVS_0-output-retriever-BaseRetriever",
                "name": "retriever",
                "label": "Retriever",
                "description": "",
                "type": "BaseRetriever"
              },
              {
                "id": "documentStoreVS_0-output-vectorStore-VectorStore",
                "name": "vectorStore",
                "label": "Vector Store",
                "description": "",
                "type": "VectorStore"
              }
            ],
            "default": "retriever"
          }
        ],
        "outputs": {
          "output": "retriever"
        },
        "selected": false
      },
      "width": 300,
      "height": 312,
      "selected": false,
      "positionAbsolute": {
        "x": -122.31978898723906,
        "y": 451.56374754134333
      },
      "dragging": false
    }
  ],
  "edges": [
    {
      "source": "chatOpenAICustom_0",
      "sourceHandle": "chatOpenAICustom_0-output-chatOpenAICustom-ChatOpenAI-Custom|BaseChatModel|BaseLanguageModel|Runnable",
      "target": "conversationalRetrievalQAChain_0",
      "targetHandle": "conversationalRetrievalQAChain_0-input-model-BaseChatModel",
      "type": "buttonedge",
      "id": "chatOpenAICustom_0-chatOpenAICustom_0-output-chatOpenAICustom-ChatOpenAI-Custom|BaseChatModel|BaseLanguageModel|Runnable-conversationalRetrievalQAChain_0-conversationalRetrievalQAChain_0-input-model-BaseChatModel"
    },
    {
      "source": "bufferWindowMemory_0",
      "sourceHandle": "bufferWindowMemory_0-output-bufferWindowMemory-BufferWindowMemory|BaseChatMemory|BaseMemory",
      "target": "conversationalRetrievalQAChain_0",
      "targetHandle": "conversationalRetrievalQAChain_0-input-memory-BaseMemory",
      "type": "buttonedge",
      "id": "bufferWindowMemory_0-bufferWindowMemory_0-output-bufferWindowMemory-BufferWindowMemory|BaseChatMemory|BaseMemory-conversationalRetrievalQAChain_0-conversationalRetrievalQAChain_0-input-memory-BaseMemory"
    },
    {
      "source": "documentStoreVS_0",
      "sourceHandle": "documentStoreVS_0-output-retriever-BaseRetriever",
      "target": "conversationalRetrievalQAChain_0",
      "targetHandle": "conversationalRetrievalQAChain_0-input-vectorStoreRetriever-BaseRetriever",
      "type": "buttonedge",
      "id": "documentStoreVS_0-documentStoreVS_0-output-retriever-BaseRetriever-conversationalRetrievalQAChain_0-conversationalRetrievalQAChain_0-input-vectorStoreRetriever-BaseRetriever"
    }
  ]
}
```

---

[← Back: Glossary](nai-appendix-glossary.md) | [Home](nai-welcome.md) | [Next: Types of Large Language Models →](nai-appendix-typellm.md)