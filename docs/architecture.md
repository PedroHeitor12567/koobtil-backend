# Arquitetura do Koobtil Backend

## Objetivo

Este documento registra as decisões arquiteturais iniciais do backend do Koobtil. Queremos um código organizado, testável e fácil de evoluir, sem adicionar complexidade desnecessária.

## Tecnologias

- **Python:** linguagem principal.
- **FastAPI:** criação da API HTTP.
- **uv:** gerenciamento do projeto, dependências e ambiente Python.
- **SQLAlchemy:** acesso ao banco de dados, quando a persistência for implementada.
- **Alembic:** migrações do banco de dados.
- **Pytest:** testes automatizados.
- **Ruff:** lint e formatação.

As dependências utilizadas devem ser registradas no `pyproject.toml` e no `uv.lock`.

## Por que Clean Architecture?

Escolhemos Clean Architecture para separar regras de negócio dos detalhes técnicos, como HTTP, banco de dados e serviços externos. Isso ajuda a:

1. **Separar responsabilidades:** cada módulo tem um propósito claro.
2. **Facilitar a manutenção:** mudanças técnicas tendem a afetar menos as regras de negócio.
3. **Testar melhor:** casos de uso podem ser testados sem iniciar o servidor ou conectar a um banco real em todos os testes.
4. **Colaborar melhor:** limites claros ajudam a dividir tarefas e revisar pull requests.
5. **Evoluir com segurança:** o sistema pode crescer sem concentrar toda a lógica nas rotas.

Clean Architecture não significa criar interfaces e classes para tudo. Adicione abstrações quando elas resolverem um problema real.

## Estrutura de referência

```text
koobtil-backend/
├── pyproject.toml
├── uv.lock
├── .python-version
├── .env.example
├── .gitignore
├── README.md
├── alembic.ini
├── alembic/
│   ├── env.py
│   └── versions/
├── src/
│   └── koobtil/
│       ├── __init__.py
│       ├── main.py
│       ├── domain/
│       │   ├── entities/
│       │   ├── exceptions/
│       │   └── repositories/
│       ├── application/
│       │   ├── use_cases/
│       │   ├── dto/
│       │   └── services/
│       ├── infrastructure/
│       │   ├── database/
│       │   │   ├── models/
│       │   │   ├── repositories/
│       │   │   └── session.py
│       │   ├── security/
│       │   └── config/
│       └── presentation/
│           └── http/
│               ├── routers/
│               ├── schemas/
│               └── dependencies.py
└── tests/
    ├── unit/
    ├── integration/
    └── e2e/
```

Não é necessário criar todas as pastas vazias de imediato. Crie os módulos conforme as funcionalidades exigirem.

## Responsabilidades das camadas

### `domain/` — domínio

Contém conceitos e regras centrais do negócio, sem depender de FastAPI, SQLAlchemy ou detalhes HTTP.

Pode conter entidades, objetos de valor, exceções do domínio e contratos de repositório.

### `application/` — aplicação

Coordena operações que o sistema oferece. Um caso de uso representa uma ação significativa, como criar um recurso ou consultar informações.

Pode conter casos de uso, DTOs de entrada/saída e serviços de aplicação.

### `presentation/http/` — apresentação

É a fronteira HTTP da aplicação. Contém rotas FastAPI, schemas Pydantic, dependências HTTP e conversão de resultados para respostas HTTP.

Mantenha as rotas pequenas: validar a entrada, chamar um caso de uso e devolver a resposta. Evite regras de negócio complexas diretamente nos endpoints.

### `infrastructure/` — infraestrutura

Implementa detalhes técnicos: modelos SQLAlchemy, sessões do banco, implementações de repositórios, integrações externas e configurações.

## Regra de dependência

As dependências do código devem apontar para dentro, em direção às regras de negócio.

Fluxo típico:
1. Uma rota recebe a requisição HTTP.
2. O schema valida os dados.
3. A rota chama um caso de uso.
4. O caso de uso aplica as regras e usa contratos internos.
5. A infraestrutura implementa esses contratos, por exemplo, acessando o banco.
6. O resultado retorna à camada HTTP.

O domínio não deve importar FastAPI, sessões SQLAlchemy ou módulos HTTP.

## Testes

- `tests/unit/`: testa regras e casos de uso isoladamente.
- `tests/integration/`: testa a integração entre componentes, como repositório e banco de teste.
- `tests/e2e/`: testa fluxos HTTP completos quando necessário.

Comece pelos testes que agregam valor; não crie testes artificiais apenas para aumentar a cobertura.

## Configuração e segredos

- Não versione o `.env`.
- Mantenha `.env.example` com nomes de variáveis e valores fictícios.
- Valide configurações na inicialização.
- Diferencie desenvolvimento, teste e produção.
- Nunca registre senhas, tokens ou dados sensíveis nos logs.

## Quando criar uma abstração?

Antes de criar uma interface, classe base ou camada nova, pergunte:
- Há uma responsabilidade diferente que precisa ser isolada?
- Uma regra de negócio está misturada com um detalhe técnico?
- A abstração facilita testes, manutenção ou troca de implementação?
- O benefício compensa a complexidade?

Se não houver ganho claro, prefira a solução mais simples.

## Decisão resumida

Escolhemos **Clean Architecture** para separar responsabilidades, facilitar testes e permitir evolução gradual. Escolhemos **uv** para simplificar o gerenciamento do projeto e ajudar toda a equipe a reproduzir um ambiente consistente.

Nosso princípio: **código simples, responsabilidades claras e decisões que facilitem a evolução do Koobtil.**
