# Промпт для задачі за розкладом (claude.ai/code/routines)

Скопіюйте текст нижче в поле **Prompt** задачі. Замініть `QA Course` на назву свого воркспейсу в Postman.

```text
Act as a Senior API QA Automation Agent.

Context:
- Postman workspace: "QA Course".
- Collection: "Swagger Petstore - Pet CRUD Workflow". Read it with the Postman connector.
- Newman is installed by the environment setup script. If `newman` is missing, run `npm install -g newman@6`.
- History of previous runs is kept in this repository on branch `claude/qa-results`, folder `results/`.

Task:
1. Fetch the collection (full model) with the Postman connector and save it as collection.json.
   If the workspace has a Postman environment for this collection, fetch it too and pass it with -e.
2. Run: newman run collection.json -r cli,json --reporter-json-export results/<UTC-timestamp>.json
3. Summarize total, passed and failed assertions, using only the Newman JSON.
4. For every failure, find the likely root cause and classify it as one of:
   API defect / test-script issue / environment issue.
5. Fetch branch `claude/qa-results` (create it if it does not exist) and compare with the
   most recent previous file in results/: new failures, fixed tests, changed counts.
6. Write a concise QA Regression Report to reports/<date>.md with recommended next actions.
   Commit the results JSON and the report to branch `claude/qa-results` and push.
7. Always send me a push notification with a 3-5 line summary
   (first line: overall status and pass/fail counts).

Rules:
- Do not modify production data, secrets, or the Postman collection without approval.
- Do not claim tests passed unless the Newman JSON confirms it.
- If a tool or the network is unavailable, report the blocker instead of inventing results.
```
