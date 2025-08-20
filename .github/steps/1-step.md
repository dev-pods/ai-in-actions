## Step 1: Introduction to AI Actions

In this exercise, you'll learn to integrate AI capabilities directly into your GitHub Actions workflows using GitHub Models. Let's start with understanding the key concepts and then you'll create and run your first AI-powered workflow!

### 📖 Theory: GitHub Models in Actions

#### 🤖 What is GitHub Models?

**GitHub Models** is a service that provides access to various AI models through an inference API. This service is available at `https://models.github.ai/inference` and allows developers to integrate AI capabilities directly into their GitHub workflows.

#### ⚙️ How GitHub Actions Work with GitHub Models

The integration between GitHub Actions and GitHub Models is designed to be seamless:

- 🔑 **Built-in Authentication**: The built-in `GITHUB_TOKEN` is used to authorize calls to the GitHub Models service, eliminating the need for additional API keys or complex authentication setup.

- 🔐 **Simple Permissions**: The `models: read` permission grants the `GITHUB_TOKEN` access to the GitHub Models inference API for making AI requests.

- 🎯 **Easy Integration**: For this exercise, we'll use the official [actions/ai-inference](https://github.com/actions/ai-inference) action, which provides a very simple path to using GitHub Models in GitHub Actions.

> [!TIP]
>
> Want to dive deeper? Check out these resources:
>
> - 📖 [GitHub Models Documentation](https://docs.github.com/en/github-models)
> - 🔗 [Using AI models with GitHub Actions](https://docs.github.com/en/github-models/use-github-models/integrating-ai-models-into-your-development-workflow#using-ai-models-with-github-actions)
> - 🔐 [GitHub Actions Permissions](https://docs.github.com/en/actions/tutorials/authenticate-with-github_token#modifying-the-permissions-for-the-github_token)



### ⌨️ Activity: Create Your First AI Workflow

Now that you understand the concepts, let's put them into practice! Open a new tab in this repository to follow these steps.

1. Navigate to your repository and make sure you're on the `main` branch.

1. Create a new workflow file at `.github/workflows/ask-ai.yml`.

1. Add the workflow metadata and permissions:

   ```yaml
   name: Ask AI
   on:
     workflow_dispatch:

   permissions:
     models: read
   ```
  
   This sets up the workflow, enables manual triggering through the GitHub UI with `workflow_dispatch`, and grants permission to access GitHub Models using the built-in `GITHUB_TOKEN`.

1. Add the job definition with AI inference steps:

   ```yaml
   jobs:
     ask-ai:
       runs-on: ubuntu-latest

       steps:
         - name: AI Inference
           id: ai-response
           uses: actions/ai-inference@v2
           with:
             prompt: |
               What is the meaning of life?

         - name: Display AI Response
           run: |
             echo "## 🤖 AI Response" >> $GITHUB_STEP_SUMMARY
             echo "" >> $GITHUB_STEP_SUMMARY
             echo "${{ steps.ai-response.outputs.response }}" >> $GITHUB_STEP_SUMMARY
   ```

   This creates a job named `ask-ai` that runs on the latest Ubuntu runner, uses the `actions/ai-inference@v2` action to send a prompt to the AI, captures the AI response with the ID `ai-response` for later reference, and displays the response in a formatted summary using GitHub's step summary feature.

1. Save your workflow file with all the content above.

1. Commit and push your workflow file to the repository.

<details>
<summary>Having trouble? 🤷</summary><br/>

- Make sure your workflow file is saved in the correct location: `.github/workflows/ask-ai.yml`
- Verify that the `models: read` permission is added at the workflow level (not job level)
- Check that you're using the correct action name: `actions/ai-inference@v2`
- If the workflow doesn't appear in the Actions tab after committing, try refreshing the page

</details>

### ⌨️ Activity: Test Your AI Workflow

Now let's test the workflow you just created to see AI in action!

1. Navigate to the **Actions** tab in your repository.

1. Look for the **Ask AI** workflow in the workflow list and **Run** it.

1. Wait for the workflow to complete and check the workflow run summary to see the AI's response displayed in a nicely formatted way.


<details>
<summary>Having trouble? 🤷</summary><br/>

- **Workflow fails to run**: Ensure you're on a repository where you have write permissions and that GitHub Actions are enabled
- **No AI response**: Make sure the `id: ai-response` is set on the AI Inference step and referenced correctly in the Display step
- **Permission errors**: Double-check that the `models: read` permission is properly configured in your workflow file
- **Action not found**: Verify you're using the exact action name: `actions/ai-inference@v2`

</details>
