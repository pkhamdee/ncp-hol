# Connect nodes

Now that our LLM is configured, we can connect our chatflow together.

1.  Link the **ChatOpenAI-Custom** output connector of the **ChatOpenAI Custom** node to the **Chat Model** input connector on the **Conversation Chain**.
    
2.  Link the **BufferWindowMemory** output connector on the **Buffer Window Memory** node to the **Memory** input connector on **Conversation Chain**.
    
    !!! tip    
        To connect the nodes, hover over the connector until the cursor turns into a +, and then drag it to the other connector.
    

Your chatflow should now look similar to the image below.

![Chatflow Example](images/chatflow-example.fe716876.png)