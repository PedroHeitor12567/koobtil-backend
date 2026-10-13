# Guia de Contribuição — Koobtil Backend

## 1. Objetivo

Este guia explica o fluxo básico para contribuir com o backend do Koobtil. Foi pensado para que iniciantes consigam participar sem precisar conhecer todo o sistema de uma vez.

## 2. Preparar o ambiente

Pré-requisitos: Git, Python na versão definida pelo projeto e uv instalado.

Clone o repositório:

```powershell
git clone <URL_DO_REPOSITORIO>
cd koobtil-backend
```

Instale as dependências do projeto:

```powershell
uv sync --dev
```

Se existir um `.env.example`, crie seu arquivo local:

```powershell
Copy-Item .env.example .env
```

Use apenas credenciais de desenvolvimento fornecidas pela equipe. Nunca envie o `.env` para o Git.

## 3. Antes de começar uma tarefa

1. Leia a issue e seus critérios de aceitação.
2. Tire dúvidas antes de começar a implementar.
3. Confira se outra pessoa já está trabalhando na tarefa.
4. Combine o escopo com a pessoa responsável pelo backend.
5. Consulte `docs/architecture.md` para decidir onde o código deve ficar.

Prefira tarefas pequenas e verificáveis. Boas primeiras tarefas incluem documentação, testes simples e correções isoladas.

## 4. Fluxo de Git

Atualize a branch principal:

```powershell
git switch main
git pull
```

Crie uma branch para sua tarefa:

```powershell
git switch -c feat/nome-curto-da-tarefa
```

Exemplos:
- `feat/create-project`
- `fix/request-validation`
- `test/project-service`
- `docs/setup-guide`

Evite desenvolver diretamente em `main`.

## 5. Durante a implementação

- Faça mudanças pequenas e coerentes.
- Respeite as responsabilidades definidas na documentação de arquitetura.
- Evite misturar grandes refatorações com a tarefa principal.
- Procure lógica existente antes de duplicá-la.
- Não adicione dependências sem justificar a necessidade.
- Não inclua credenciais, tokens ou dados sensíveis no código, commits ou logs.
- Atualize os testes quando o comportamento mudar.
- Atualize a documentação quando alterar comandos ou configurações.

Se não souber onde colocar algo, descreva a responsabilidade do código e peça uma revisão antes de criar uma nova camada.

## 6. Verificações antes do commit

Execute:

```powershell
uv run pytest
uv run ruff check .
uv run ruff format --check .
```

Para formatar o código:

```powershell
uv run ruff format .
```

Se algum comando ainda não estiver configurado ou algum teste depender de um serviço externo, informe isso no pull request.

## 7. Commits

Use mensagens objetivas. O projeto pode adotar Conventional Commits:

```text
feat: add project creation endpoint
fix: validate project request
test: cover project creation use case
docs: explain local setup
refactor: simplify request handling
```

Evite mensagens vagas como `ajustes`, `final` ou `corrigindo tudo`.

## 8. Pull request

Abra um pull request da sua branch para `main`. Inclua objetivo, alterações, como testar e pontos pendentes.

Modelo:

```markdown
## Objetivo
Qual problema esta PR resolve?

## Alterações
- Liste as principais mudanças.

## Como testar
- `uv run pytest`
- Inclua passos manuais, se necessários.

## Checklist
- [ ] Segui a arquitetura do projeto.
- [ ] Adicionei ou atualizei testes quando necessário.
- [ ] Executei lint e formatação.
- [ ] Não incluí segredos.
- [ ] Atualizei a documentação, se necessário.
```

## 9. Code review

Quem revisa deve verificar se a mudança resolve o problema, atende aos critérios, está no lugar correto da arquitetura, é legível e tem testes relevantes.

Faça comentários específicos e respeitosos, explicando o motivo da sugestão. O review é também uma oportunidade de aprendizado.

## 10. Definition of Done

Uma tarefa está concluída quando:
- [ ] Os critérios de aceitação foram atendidos.
- [ ] O código respeita a arquitetura.
- [ ] Testes relevantes foram criados ou atualizados.
- [ ] Testes e verificações foram executados, ou limitações foram documentadas.
- [ ] Nenhum segredo foi incluído.
- [ ] A documentação foi atualizada quando necessário.
- [ ] O pull request foi revisado e aprovado.
- [ ] A mudança foi integrada conforme o fluxo da equipe.

## 11. Como pedir ajuda

Ao pedir ajuda, informe:
1. O que estava tentando fazer.
2. O que esperava que acontecesse.
3. O que aconteceu de fato.
4. A mensagem de erro completa, removendo tokens e dados sensíveis.
5. O comando executado e os passos para reproduzir.

Você não precisa chegar com a solução pronta. Mostre o que tentou e onde ficou bloqueado.
