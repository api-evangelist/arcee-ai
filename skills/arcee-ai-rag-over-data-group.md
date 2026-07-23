---
name: Retrieval-augmented answering over an uploaded data group
description: Create a data group, upload documents to it, and ask questions grounded in those documents via the Arcee Platform RAG endpoint.
api: openapi/arcee-ai-afm-openapi.json
operations:
  - create_data_group_app_v1_data_groups_post
  - upload_files_app_v1_files__data_group_id__post
  - get_top_k_app_v1_v1_top_k__data_group_id__get
  - rag_app_v1_rag_post
---

# Retrieval-augmented answering over an uploaded data group

Ground model answers in your own documents using Arcee Platform **data groups** and the RAG
endpoint. Base URL: `https://api.arcee.ai`. Auth: `Authorization: Bearer <API_KEY>`
(see `authentication/arcee-ai-authentication.yml`).

## Steps

1. **Create a data group** — `create_data_group_app_v1_data_groups_post`
   (`POST /app/v1/data-groups`). Keep the returned `data_group_id`.
2. **Upload documents** — `upload_files_app_v1_files__data_group_id__post`
   (`POST /app/v1/files/{data_group_id}`) with the files to index. List them back with
   `get_files_app_v1_files__data_group_id__get`.
3. **(Optional) Inspect retrieval** — `get_top_k_app_v1_v1_top_k__data_group_id__get`
   (`GET /app/v1/v1/top-k/{data_group_id}`) to preview the top-k chunks a query retrieves.
4. **Ask a grounded question** — `rag_app_v1_rag_post` (`POST /app/v1/rag`) referencing the data
   group; the response is generated over the retrieved context.

## Notes
- Clean up with `delete_files_app_v1_files__data_id___data_group_id__delete` and
  `delete_data_group_app_v1_data_groups__group_id__delete`.
- Errors follow the `{"detail": ...}` envelope; `422` means a bad parameter (inspect `detail[].loc`).
  See `errors/arcee-ai-problem-types.yml`.
