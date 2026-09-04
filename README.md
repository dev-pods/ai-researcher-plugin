# AI Researcher Plugin

Pacote de agentes para pesquisa, síntese de fontes e escrita de documentação.

## Objetivo

Centralizar o fluxo de trabalho em um orquestrador chamado `ai-researcher` e separar a busca por especialidade, para reduzir a deriva de contexto e facilitar a manutenção de cada fonte.

## Requisitos

 - Extensão `ms-vscode.vscode-websearchforcopilot`

## VS Code Chat Plugin

Instale este plugin pelo ponto de entrada de Chat Plugins do VS Code:

[![Install Chat Plugin VS Code](https://img.shields.io/badge/Install_Chat_Plugin-VS_Code-blue)](vscode://chat-plugin/install?source=dev-pods/ai-researcher-plugin)

[![Install Chat Plugin VS Code Insiders](https://img.shields.io/badge/Install_Chat_Plugin-VS_Code_Insiders-24BFA5)](vscode-insiders://chat-plugin/install?source=dev-pods/ai-researcher-plugin)

## Estrutura

- `plugin.json` - manifesto canônico do plugin
- `mcp.json` - configuração canônica dos servidores MCP
- `.plugin/plugin.json` - symlink de compatibilidade para `../plugin.json`
- `.mcp.json` - symlink de compatibilidade para `mcp.json`
- `agents/ai-researcher.agent.md` - orquestrador principal
- `agents/ai-researcher-github.agent.md` - especialista em GitHub docs
- `agents/ai-researcher-microsoft.agent.md` - especialista em Microsoft docs
- `agents/ai-researcher-claude-code.agent.md` - especialista em Claude Code docs
- `agents/ai-researcher-openai.agent.md` - especialista em OpenAI docs
- `agents/ai-researcher-web.agent.md` - especialista em Web Search for Copilot
- `agents/ai-researcher-search-view.agent.md` - especialista no Search view atual do VS Code
- `agents/ai-researcher-writer.agent.md` - especialista em redação de documentação
- `com.github.copilot/agents/` - symlinks dos agentes para o layout empacotado do Copilot

As definições em `agents/`, o manifesto `plugin.json` e a configuração
`mcp.json` são as únicas fontes canônicas. Os layouts alternativos usam
symlinks relativos, portanto não exigem sincronização manual de conteúdo.

## Fluxo

1. O orquestrador refina a pergunta do usuário.
2. Em seguida, ele pergunta onde a resposta deve ser buscada.
3. As opções são:
   - github_docs - GitHub Docs (MCP)
   - microsoft_docs - Microsoft Docs (MCP)
   - claude-code-docs - Claude Code Docs (MCP)
   - openai-docs - OpenAI Docs (MCP)
   - web_search_for_copilot - Web Search for Copilot (VS Code Extension)
   - get-search-view-results - Search view atual do VS Code (VS Code Skill, caminho especial)
4. Cada fonte delega para um agente especialista.
5. O agente de escrita transforma os achados em rascunho pronto para documentação.

Notas:
- Fontes MCP são configuradas em `mcp.json`; `.mcp.json` é apenas um alias simbólico.
- Ferramentas do VS Code não são servidores MCP e não devem ser adicionadas em `mcp.json`.
- Para usar `web_search_for_copilot`, instale/ative a extensão `ms-vscode.vscode-websearchforcopilot` (recomendada em `.vscode/extensions.json`).

## Manutenção

Ao adicionar um agente, crie sua definição canônica em `agents/` e um symlink
com o mesmo nome em `com.github.copilot/agents/`, apontando para
`../../agents/<arquivo>.agent.md`. Mantenha a versão do plugin em `plugin.json`
alinhada às mudanças publicadas e revise os servidores declarados em `mcp.json`.

## Status

Esta implementação cobre o pacote de agentes, a configuração MCP e as recomendações de extensões necessárias.
