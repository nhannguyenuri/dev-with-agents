## Strict Scope & Execution Rules

1. Follow ONLY the explicit task requested in the user's prompt. Do NOT proactively explore, search, read, or analyze the codebase unless explicitly asked.
2. If a task only asks to read/fetch/summarize external information (such as an Azure DevOps work item/bug), do NOT touch repository files, run search/grep on code, or propose code changes unless asked.
3. If ambiguity exists or extra context from the codebase might be helpful, STOP and ask for confirmation first instead of assuming permission to inspect the code.
4. Keep output strictly aligned with the requested deliverable (e.g., summary only). Do not append unsolicited root-cause guesses or implementation plans based on unrequested code scans
