## Step 2

Great work! Your first AI workflow is now functional. Next, let's see how to combine the `ai-inference` action with other actions to create meaningful AI workflows for your projects.

### 📖 Theory: Composing AI Workflows

AI adds the most value in Actions when you connect three pieces:

```mermaid
graph LR
    A[📥 Context] --> B[🤖 AI Inference]
    B --> C[📤 Impact]

    style A fill:#4fc3f7,stroke:#333,stroke-width:2px,color:#000
    style B fill:#ffb74d,stroke:#333,stroke-width:2px,color:#000
    style C fill:#ba68c8,stroke:#333,stroke-width:2px,color:#000
```

- 📥 **Context**: Gather data from outputs of other actions (for example, file contents, API results, or computed values) or `github` [event context](https://docs.github.com/en/actions/reference/workflows-and-actions/contexts#github-context)
- 🤖 **AI Inference**: Take the context and build a focused prompt for `actions/ai-inference` to analyze it
- 📤 **Impact**: Pass the AI result to another action to create impact.

This pattern keeps workflows simple while handling judgment‑heavy tasks that are hard to script deterministically.

> [!TIP]
>
> Want to dive deeper? Check out these resources:
>
> - 📖 [GitHub Actions Context](https://docs.github.com/en/actions/learn-github-actions/contexts)
> - 🔧 [Workflow Inputs](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#onworkflow_dispatchinputs)
> - 🤖 [AI Inference System Prompts](https://github.com/actions/ai-inference#system-prompts)

### ⌨️ Activity: Create an issue analyzer workflow

1. Create `.github/workflows/issue-analyzer.yml` workflow

Add the workflow metadata and permissions

```yaml
name: Issue Analyzer

on:
  issues:
    types: [opened]

permissions:
  models: read
  issues: write
```

This enables the workflow on new issues and grants access to GitHub Models plus permission to write comments.

1. Add the job with AI inference:

   Append the following to the same file to create the job and analyze the issue content with `ai-inference`:

   ```yaml
   jobs:
     analyze:
       name: AI Issue Analyzer
       runs-on: ubuntu-latest
       steps:
         - name: Analyze issue with AI
           id: ai-response
           uses: actions/ai-inference@v2
           with:
             system: |
               You are an assistant that triages GitHub issues. Summarize the issue, identify missing information and propose next steps. Be concise and actionable.
             prompt: |
               New issue was opened by ${{ github.event.issue.user.login }}
               Title: ${{ github.event.issue.title }}
               Body:
               ---
               ${{ github.event.issue.body }}
               ---
   ```

1. Add the commenting step:

   Use the AI output file to post a comment back to the issue:

   ```yaml
         - name: Comment results on the issue  # 🤖 AI Inference → 📤 Impact
           uses: peter-evans/create-or-update-comment@v4
           with:
             token: ${{ secrets.GITHUB_TOKEN }}
             issue-number: ${{ github.event.issue.number }}
             body-path: ${{ steps.ai-response.outputs.response-file }}
   ```

1. Commit the file to the default branch, then open the **Actions** tab and confirm the workflow appears.

<details>
<summary>Having trouble? 🤷</summary><br/>

- Ensure Actions are enabled for the repository.
- Check permissions: the workflow needs `models: read` and `issues: write`.
- If no comment appears, open the run logs. Verify `ai-inference` produced a `response-file` output.
- If the model name is restricted in your org, switch `model` to an allowed option.
- Very short or empty issue bodies can reduce output quality—try a more descriptive issue.

</details>

### ⌨️ Activity: Test the workflow

1. Navigate to the Issues tab and click **New issue**. Use the title `AI: test issue analyzer` and the following body:

   ```markdown
   Description
   A user reports intermittent 500 errors after submitting the login form. Errors occur more often on mobile.

   Steps to reproduce

   1. Open the site on a mobile device (Pixel 7, Chrome 126)
   2. Go to /login
   3. Enter valid credentials and submit

   Expected
   Successful redirect to the dashboard.

   Actual
   Occasional 500 error with generic message. No client-side errors in console.
   ```

1. Check that your workflow was triggered when opening the issue (see the **Actions** tab or the issue timeline).

1. When the workflow completes, you should see a new comment on the issue with the analysis and next steps.

<details>
<summary>Having trouble? 🤷</summary><br/>

- If the workflow didn’t run, confirm the trigger is `issues: [opened]` and you created a new issue (not edited an existing one).
- If the comment failed, check the job’s final step for API errors (permissions, rate limits, or missing `GITHUB_TOKEN`).
- If the response is empty or truncated, try simplifying the issue body or adjusting the prompt format.

</details>
