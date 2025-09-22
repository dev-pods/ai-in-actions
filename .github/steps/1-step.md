## Passo 1: Introdução às AI Actions

Neste exercício, você aprenderá a integrar capacidades de IA diretamente nos seus workflows do GitHub Actions usando o GitHub Models. Vamos começar entendendo os conceitos principais e depois partir direto para a criação do seu primeiro workflow com IA!

### 📖 Teoria: GitHub Models em Actions

#### 🤖 O que é o GitHub Models?

**[GitHub Models](https://docs.github.com/github-models)** é um serviço que fornece um catálogo curado de modelos de IA de fornecedores líderes. Entre seus muitos casos de uso, o GitHub Models inclui uma API de inferência disponível em `https://models.github.ai/inference` que permite aos desenvolvedores integrar capacidades de IA diretamente nos seus workflows e aplicações do GitHub.

#### ⚙️ Como o GitHub Actions funciona com o GitHub Models

A [integração](https://docs.github.com/en/github-models/use-github-models/integrating-ai-models-into-your-development-workflow#using-ai-models-with-github-actions) entre GitHub Actions e GitHub Models é projetada para ser perfeita:

- 🔑 **Autenticação Integrada**: O [`GITHUB_TOKEN`](https://docs.github.com/en/actions/tutorials/authenticate-with-github_token#modifying-the-permissions-for-the-github_token) integrado do GitHub Actions pode ser usado para autorizar chamadas ao serviço GitHub Models, eliminando a necessidade de chaves de API adicionais ou configuração complexa de autenticação com provedores terceiros.

- 🔐 **Permissões Simples**: A permissão [`models: read`](https://docs.github.com/en/actions/tutorials/authenticate-with-github_token#modifying-the-permissions-for-the-github_token) concede ao `GITHUB_TOKEN` acesso à API de inferência do GitHub Models para fazer solicitações de IA.

- 🎯 **Integração Fácil**: A action oficial [actions/ai-inference](https://github.com/actions/ai-inference) fornece um caminho muito simples para usar o GitHub Models no GitHub Actions.

> [!TIP]
>
> Quer se aprofundar mais? Confira estes recursos:
>
> - 📖 [Documentação do GitHub Models](https://docs.github.com/en/github-models)
> - ⚡ [Limites de Taxa](https://docs.github.com/en/github-models/use-github-models/prototyping-with-ai-models#rate-limits) e [Indo Além dos Limites Gratuitos](https://github.blog/changelog/2025-06-24-github-models-now-supports-moving-beyond-free-limits/) para GitHub Models

### ⌨️ Atividade: Crie Seu Primeiro Workflow de IA

Agora que você entende os conceitos, vamos colocá-los em prática! Abra uma nova aba deste repositório para seguir estes passos.

Vamos criar um workflow simples que podemos acionar manualmente pela interface do GitHub.

1. Navegue até a aba `Code` do seu repositório. Em seguida, entre no diretório `.github/workflows/`.

1. Clique em `Add File` e crie um novo arquivo de workflow chamado

   ```text
   ask-ai.yml
   ```

1. Comece adicionando o nome do workflow, trigger de evento manual e as permissões necessárias:

   ```yaml
   name: Ask AI
   on:
     workflow_dispatch:

   permissions:
     models: read
   ```

   > ❗ **Atenção:** Copie o conteúdo conforme fornecido, pois este nome exato do workflow (`Ask AI`) é necessário para progredir para os próximos passos deste exercício.

1. Agora vamos adicionar um job que usa a action de inferência de IA.

   Neste cenário simples, faremos uma pergunta simples codificada para a IA e exibiremos a resposta no resumo do workflow:

   ```yaml
   jobs:
     ask-ai:
       runs-on: ubuntu-latest

       steps:
         - name: AI Inference
           id: ai-response
           uses: actions/ai-inference@v2
           with:
             token: {% raw %}${{ secrets.GITHUB_TOKEN }}{% endraw %}
             prompt: |
               Me conte uma piada de programação.

         - name: Display AI Response
           run: |
             echo "## 🤖 Resposta da IA" >> $GITHUB_STEP_SUMMARY
             echo "" >> $GITHUB_STEP_SUMMARY
             echo "{% raw %}${{ steps.ai-response.outputs.response }}{% endraw %}" >> $GITHUB_STEP_SUMMARY
   ```

   > ❗ **Atenção:** Fique atento à formatação YAML! O editor de arquivos do GitHub mostrará sublinhados vermelhos para certos erros de YAML.

1. Faça commit do arquivo de workflow diretamente na branch `main`.

### ⌨️ Atividade: Teste Seu Workflow de IA

Agora vamos testar o workflow que você acabou de criar para ver a IA em ação!

1. Navegue até a aba **Actions** no seu repositório.

1. Na barra lateral esquerda, procure pelo workflow **Ask AI** na lista de workflows e clique nele.

1. Clique no botão **Run workflow**, mantenha a branch padrão selecionada e clique no botão verde **Run workflow** para acioná-lo.

   <img width="900" alt="executar workflow com trigger manual" src="https://github.com/user-attachments/assets/89d96ce7-ca5e-4f5f-b8d0-25ebd5cdc4d6" />

1. Aguarde o workflow ser concluído e verifique o resumo da execução do workflow para ver a resposta da IA exibida de forma bem formatada.

1. Conforme seu workflow é concluído com sucesso, Mona preparará automaticamente o próximo passo na sua jornada de aprendizado!

<details>
<summary>Tendo problemas? 🤷</summary><br/>

- **Workflow falha ao executar**: Certifique-se de que o workflow esteja completo e com formatação yaml adequada, se não estiver então:
  - Encontre o problema no workflow e faça commit das mudanças novamente na branch `main`
  - Tente executar o workflow novamente
- **Sem resposta da IA**: Certifique-se de que o `id: ai-response` esteja definido no step AI Inference e referenciado corretamente no step Display
- **Erros de permissão**: Verifique novamente se a permissão `models: read` está configurada adequadamente no seu arquivo de workflow
- **Action não encontrada**: Verifique se você está usando o nome exato da action: `actions/ai-inference@v2`

</details>
