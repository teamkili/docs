# Stream answers

This API endpoint allows you to stream a generated answer to a question based on the context you have ingested into Kili. The endpoint allows you to maintain full history of the conversation.

## Endpoint

- POST https://api.kili.so/v1/stream-answer

## Authentication

Authentication is done using a bearer token. Include the bearer token in the "Authorization" header of the request to verify your identity and access the API. Send this information as follows:

```
{
  "Authorization": "Bearer [YOUR_API_KEY]"
}
```

## Request

**Required:**

- project_id (string): The ID of your project. Provide a unique identifier for your project.
- question (string): The question you want answered, for example: how do I sign up?

**Optional:**

- chat_thread_id (string): A string identifier to store the original chat ID. If not provided, we will automatically create one for you and send back in the response. If included and we find a matching ID, we will resume that specific chat.

**Sample request:**

```
{
  "project_id": "19a365f4-504a-4fe5-9a2d-66f40ee4581e",
  "chat_thread_id": "31a67fb9-70f5-4638-bfae-c0c9f4e3600c",
  "question": "How do I get a refund on my account?",
}
```

## Response

The response is streamed through server-sent events(SSE). A series of message events are sent, here's what they look like:

1. `message` is a part of the generated response sent in-order of generation. After all the `message` events are sent, the next two types are sent in sequence.

2. `chat_id` and `chat_thread_id` are unique ID used to identify this chat. This can be used to persist the chat history.

3. `result` which includes the context associated with the generated answer to your question and some extra information.

**Parameters:**

- message:

  - message (string): A part of the generated response

- chat_id (string)
- chat_thread_id (string)

- results:
  - id (string): a unique id for this piece of content
  - title (string): title of the source
  - class_name (string): the type of source
  - content (string): content of the source. This context is embedded and retreived based on the user's question. If you've
  - tokens (int): number of tokens used.
  - url (string): url for the source, if available.

**Sample response:**

You'll first recieve several events with `message` JSON like so:

```
{
  "message": "example"
}
```

Then followed by the `chat_id` and `chat_thread_id` JSON:
{
"chat_id": "e7d659c6-2g55-4e21-9b97-9813er1c9b14",
"chat_thread_id": "f7d359g6-3g5g-te28-1b87-9813er1c9b14"
}

For each piece of context used to generate the answer, a separate event with `result` JSON is sent:

```
"result": {
  "id": "d25a49b3-f57f-4fa3-937f-8ab356c5e605",
  "title": "How to get a refund?",
  "class_name": "Document",
  "content": "Customers can request refunds by going to Settings > My Account and clicking on 'Refund'. They need to confirm the account they want to receive money at. Refunds are processed within 24 hours."
  "tokens": 14,
  "url": "https://example.com/help/refunds",
}
```
