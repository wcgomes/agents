# agents

Workspace de exemplo para trabalhar com agentes de IA. Define regras de comportamento, habilidades reutilizáveis e uma base de conhecimento persistente para guiar agentes em qualquer tarefa.

---

## Estrutura

```
AGENTS.md        # Regras de comportamento do agente — lido antes de toda tarefa
skills/          # Habilidades carregadas sob demanda
│   ├── delegate/    # Delega tarefas a agentes especialistas
│   ├── wiki/        # Gerencia a base de conhecimento do workspace
│   └── workflow/    # Guia planejamento, execução e verificação
wiki/            # Memória persistente do workspace (criada sob demanda)
```

---

## Principais recursos

### `AGENTS.md`
Instruções permanentes para o agente. Define:
- **Think Before Acting** — assumir explicitamente, perguntar quando incerto.
- **Minimum Words** — cada palavra carrega informação; cortar o resto.
- **Minimum Viable Change** — menor mudança que resolve o problema.
- **Surgical Changes** — não melhorar código adjacente; combinar estilo existente.

### Skills
Habilidades modulares ativadas quando relevantes:

| Skill | Função |
|---|---|
| `delegate` | Identifica agentes especialistas e delega; por padrão antes de toda tarefa |
| `wiki` | Query, ingest e lint da base de conhecimento em `wiki/` |
| `workflow` | Define critérios de sucesso, coordena execução, gerencia aprovação |

### Wiki
Base de conhecimento persistente em `wiki/`. Auto-aprendizado — cada tarefa deve deixar o workspace mais documentado. Uma página por conceito; `index.md` obrigatório e sempre atualizado.
