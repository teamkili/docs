# Generate answers

This API endpoint allows you to generate an answer to a question based on the context you have ingested into Kili. The endpoint allows you to maintain full history of the conversation.

## Endpoint

- POST /api/v1/answer

## Authentication

Authentication is done using a bearer token. Include the bearer token in the "Authorization" header of the request to verify your identity and access the API. Send this information as follows:
{
"Authorization": "Bearer [YOUR_API_KEY]"
}

## Request

**Required:**

- project_id (string): The ID of your project. Provide a unique identifier for your project.
- question (string): The question you want answered, for example: how do I sign up?

**Optional:**

- history (array): An array of chat history objects representing the conversation. Each object in the array should have the following properties:
  - role (string): The role of the participant in the conversation. Use "user" for user messages and "assistant" for assistant messages.
  - content (string): The content of the message. Enter the text of the message for each role.
- chat_id (string): A string identifier to store the original chat ID. Provide a unique ID for the chat session.

**Sample request:**

```
{
	"project_id": "19a365f4-504a-4fe5-9a2d-66f40ee4581e",
	"chat_id": "31a67fb9-70f5-4638-bfae-c0c9f4e3600c",
	"question": "Do you sell red shoes?",
	"history":[
			{
        "role": "user",
        "content": "Do you sell shoes?"
      },
      {
        "role": "assistant",
        "content": "Yes, we formal and casual shoes. We have a range of colors available to suit your needs."
      }
		]
}
```

## Response

**Parameters:**

- answer (string): Response to your question.
- results (array): An array of result objects that have the following properties. A subset of these results are used to generate the answer.
  - id: a unique idea for the content
  - source_id: the source id for this piece of content
  - title: title of the source
  - url: url for the source, if available.
  - content: content of the source. This context is embedded and retreived based on the user's question. If no metadata is provided, this information is used to generate an answer.
  - metadata: any metadata that was supplied for this source. IF available, this metadata is used in the context to generate the response.
- tokens (int): number of tokens used for generating the answer.
- chat_id (string): unique ID used to identify this chat. Use this variable to persist chat history.

**Sample response:**

```
{
	"answer": "Yes, we sell red shoes. Would you like to take a look at them?",
	"results": [
    {
      "id": "d25a49b3-f57f-4fa3-937f-8ab356c5e605",
      "source_id": "a0e29a7b-2f56-4f3c-96f2-77f90b9db67e",
      "title": "Red Shoes 1",
      "url": "https://example.com/red-shoes-1",
      "content": "Red Running Shoes - $49.99 - Lightweight and Comfortable - Perfect for Running and Sports",
      "metadata": {
        "id": "6d327b55-0d4b-4a9b-a269-16d7b0ad2b69",
        "name": "Red Running Shoes",
        "image": "https://example.com/images/red-shoes-1.jpg",
        "price": "$49.99",
        "title": "Lightweight and Comfortable",
        "description": "Perfect for Running and Sports"
      }
    },
    {
      "id": "e7d689c6-2855-4e21-9b97-9813ed1c7b14",
      "source_id": "c88a6348-906e-4459-bc1e-6d704536c0a0",
      "title": "Red Shoes 2",
      "url": "https://example.com/red-shoes-2",
      "content": "Red Sneakers - $39.99 - Trendy and Stylish - Ideal for Casual Wear",
      "metadata": {
        "id": "c2f30323-30cc-43d1-a5a5-0d01acac0489",
        "name": "Red Sneakers",
        "image": "https://example.com/images/red-shoes-2.jpg",
        "price": "$39.99",
        "title": "Trendy and Stylish",
        "description": "Ideal for Casual Wear"
      }
    }
  ],
  tokens: 456,
  chat_id: "e7d659c6-2g55-4e21-9b97-9813er1c9b14"
}
```
