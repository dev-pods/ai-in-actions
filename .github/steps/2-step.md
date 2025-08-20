## Step 2: Using Dynamic Context

Great work! Your AI workflow is now functional. Next, let's make it more interactive and powerful by adding dynamic context that allows users to provide input and gives the AI more personality.

### 📖 Theory: Evolving AI Workflows with Prompt Engineering

As AI workflows mature, they evolve from simple, hardcoded interactions to sophisticated, reusable tools. This progression involves two key improvements: making workflows interactive through user inputs and refining AI behavior through prompt engineering.

**Workflow Evolution Stages:**

- **Basic**: Hardcoded prompts with fixed questions (like your Step 1 workflow)
- **Interactive**: User-driven inputs that make workflows reusable across different scenarios
- **Intelligent**: System prompts that give AI consistent personality and specialized expertise

**Prompt Engineering Fundamentals:**

- **System prompts** define the AI's role, personality, and response style - they set the "character" of your AI assistant
- **User prompts** contain the actual questions or tasks - they provide the specific content to process
- **Separation of concerns** allows you to maintain consistent AI behavior while handling varied user requests
- **Persona design** creates engaging, memorable AI interactions that users want to return to

This combination transforms a basic AI workflow into a specialized tool that teams can use repeatedly, with the AI maintaining its unique personality while addressing different questions and scenarios.

Learn more about [GitHub Actions Context](https://docs.github.com/en/actions/learn-github-actions/contexts), [Workflow Inputs](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#onworkflow_dispatchinputs), and [AI Inference System Prompts](https://github.com/actions/ai-inference#system-prompts).

### ⌨️ Activity: Enhance with Dynamic Context

1. Update your existing `.github/workflows/ask-ai.yml` file to make it more interactive

1. Add an input parameter to the workflow_dispatch trigger:

   ```yaml
   on:
     workflow_dispatch:
       inputs:
         question:
           description: "Question to ask"
           required: true
           type: string
   ```

   This adds an input field to the workflow so users can ask custom questions instead of the hardcoded one.

1. Update the AI inference step to include a system prompt and use the dynamic input:

   ```yaml
   - name: AI Inference
     id: ai-response
     uses: actions/ai-inference@v2
     with:
       system-prompt: |
         You are a pirate AI assistant who speaks like a seasoned sailor from the 1700s. 
         Always begin your responses with "Ahoy matey!" and use pirate terminology throughout.
         Replace common words with pirate speak: "yes" becomes "aye", "you" becomes "ye" etc.
         End every response with a pirate exclamation like "Arrr!" or "Shiver me timbers!"
         While maintaining your pirate persona, still provide accurate and helpful information.
       prompt: |
         ${{ inputs.question }}
   ```

   This adds personality to the AI with a system prompt and uses the user's input question instead of a hardcoded prompt.

1. Commit and push your updated workflow file

### ⌨️ Activity: Test Dynamic Context

1. Navigate to the Actions tab and find your "Ask AI" workflow

1. Click "Run workflow" and notice the new input field for your question of your choosing

1. Check the workflow run summary to see the AI's response displayed

<details>
<summary>Having trouble? 🤷</summary><br/>

- Make sure the workflow input is defined under `on.workflow_dispatch.inputs`
- Verify that the input name matches exactly what you're referencing with `${{ inputs.question }}`
- Check that the `system-prompt` parameter is added to the `actions/ai-inference` step
- If the input field doesn't show up, ensure your workflow file is properly committed and pushed

</details>
