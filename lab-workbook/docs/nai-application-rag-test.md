# Test the Chatbot

Before testing our new chatbot, let's ask a specific question to our existing chatbot.

1.  If you're currently on the chatflow canvas, click the **<** arrow to return to the chatflow list.
    
    ![Back Arrow](images/back-arrow.7caec246.png)
    
2.  From the chatflow list, click on your original chatflow.
    
3.  Open up the chatbot by clicking on the chat icon.
    
    ![Chat Icon](images/chat-icon.1529c512.png)
    
4.  Ask the question `What are the new NVIDIA NIMs supported in NAI 2.5?` and view the output.
    
    Since this chatbot is not using our document store, it is relying on its general knowledge, and will give a generic and inaccurate answer.
    
    ![Answer Without RAG](images/answer-without-rag.bc3235dc.png)
    
5.  Click the back arrow and click on your new RAG-enabled chatflow.
    
6.  Open up the chatbot by clicking on the chat icon.
    
7.  Ask the same question `What are the new NVIDIA NIMs supported in NAI 2.5?` and view the output. The answer should be referenced from the document we uploaded. You can view the referenced document chunks at the end of the output.
    
    ![Answer With RAG](images/answer-with-rag.faa6d164.png)