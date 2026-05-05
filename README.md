# Atlas - Frontend Service

Interface de usuário do ecossistema de microsserviços Atlas, responsável pela experiência web do sistema de gerenciamento de bolsas de estudo. Este projeto foi estruturado com foco em escalabilidade, manutenção e padronização visual para evoluir em conjunto com os demais serviços da plataforma.

## ✨ Tech Stack

| Tecnologia | Papel no projeto |
| --- | --- |
| React 18 | Base da interface reativa e componentizada |
| Vite | Build tool e ambiente de desenvolvimento com Hot Reload |
| TypeScript | Tipagem estática para reduzir erros e melhorar a manutenção |
| Docker | Padronização do ambiente de execução local |
| Atomic Design | Organização dos componentes de UI em camadas reutilizáveis |

## 🏗️ Arquitetura de Pastas

A pasta `src` concentra toda a implementação da interface. A organização foi pensada para separar responsabilidades e tornar a evolução do frontend previsível.

| Pasta | Responsabilidade |
| --- | --- |
| `src/components` | Biblioteca de componentes reutilizáveis, organizada por Atomic Design |
| `src/pages` | Telas de rota, responsáveis por compor a experiência de cada página |
| `src/hooks` | Hooks customizados para encapsular lógica reutilizável de estado, efeitos e integração |
| `src/services` | Camada de acesso a APIs e integração com o backend |
| `src/assets` | Imagens, ícones, ilustrações e demais recursos estáticos |

### `src/components` e Atomic Design

O diretório `src/components` deve seguir rigorosamente a metodologia Atomic Design.

| Camada | Objetivo | Exemplos |
| --- | --- | --- |
| `atoms` | Menores blocos de interface, com responsabilidade única | botões, inputs, labels, ícones |
| `molecules` | Combinações simples de átomos que formam um comportamento coeso | campos com label e mensagem de erro, cards resumidos |
| `organisms` | Seções mais completas da interface, compostas por moléculas e átomos | cabeçalhos, formulários complexos, listas com filtros |
| `templates` | Estruturas de página sem dependência de dados de negócio | layouts base, colunas, shells visuais |

Regras práticas para essa organização:

- Componentes de baixo nível devem permanecer desacoplados de regras de negócio.
- A composição deve acontecer de baixo para cima, do átomo ao template.
- Telas finais não devem “furar” a hierarquia e importar diretamente detalhes internos quando existir um componente apropriado.
- Novos componentes devem ser classificados na camada correta antes de serem adicionados ao repositório.

## 🐳 Setup com Docker

### Pré-requisitos

| Requisito | Observação |
| --- | --- |
| Docker Engine / Docker Desktop | Necessário para construir e executar o container |
| Docker Compose | Usado para subir o serviço `frontend` |
| Node.js | Não é obrigatório para executar via container, mas ajuda em tarefas locais |

### Execução do zero

1. Acesse a pasta do frontend:

```bash
cd frontend-service
```

2. Garanta que o arquivo `.env.development` exista ou crie um `.env` com as variáveis necessárias.

3. Suba a aplicação com build da imagem:

```bash
docker compose up --build
```

4. Acesse a interface no navegador:

```text
http://localhost:5173
```

### Nota importante para Windows

Para usuários de Windows, recomenda-se fortemente:

- Usar WSL 2 como backend do Docker Desktop.
- Executar o Docker Desktop e o terminal como Administrador, especialmente na primeira configuração.
- Verificar se o compartilhamento de arquivos e a integração com a distro WSL estão habilitados.

Esses passos evitam problemas comuns de performance, montagem de volumes e comunicação entre o host Windows e o container.

## ⚙️ Configuração de Ambiente

O frontend utiliza variáveis de ambiente com o prefixo `VITE_`, conforme o padrão do Vite.

### Arquivo `.env`

Crie ou ajuste o arquivo de ambiente dentro de `frontend-service` com base no exemplo do projeto.

```env
VITE_API_URL=http://localhost:8000/api
VITE_ENV_NAME=development
```

### Diretrizes

- `VITE_API_URL` deve apontar para o backend service responsável pelos dados da aplicação.
- Dados sensíveis nunca devem ser hardcoded no frontend.
- Segredos, tokens e credenciais devem ficar no backend ou em mecanismos seguros de configuração.
- Variáveis usadas no código precisam começar com `VITE_` para ficarem disponíveis no bundle do Vite.

## 🔁 Fluxo de Desenvolvimento

### Adicionando novas dependências

Quando uma dependência for adicionada ao projeto, o container deve ser reconstruído para garantir que o ambiente reflita o novo estado do `package.json` e do lockfile.

```bash
docker compose up --build
```

Se você alterar dependências dentro do container ou atualizar o lockfile, prefira reconstruir a imagem em vez de depender apenas do container já em execução.

### Hot Reload com volumes do Docker

O `docker-compose.yml` monta o código-fonte como volume:

```yaml
volumes:
	- .:/app
	- /app/node_modules
```

Isso permite que mudanças feitas no código sejam refletidas imediatamente no navegador durante o desenvolvimento, aproveitando o Hot Reload do Vite.

Em resumo:

- Alterou um arquivo em `src`? O container recebe a mudança via volume.
- Salvou o arquivo? O Vite recompila apenas o necessário.
- Evitou reinstalar dependências a cada ciclo? O volume separado de `node_modules` ajuda a preservar o ambiente do container.

## 🧭 Padrões de Código

O projeto segue uma base de qualidade voltada para clareza, previsibilidade e manutenção contínua.

| Padrão | Diretriz |
| --- | --- |
| ESLint | A base de lint deve ser respeitada antes de qualquer merge |
| Clean Code | Nomes claros, funções pequenas e responsabilidade única |
| Atomic Design | Obrigatório para novos componentes de UI |
| TypeScript | Tipos explícitos quando agregarem legibilidade e segurança |

### Boas práticas esperadas

- Prefira composição em vez de duplicação.
- Mantenha componentes com responsabilidade única.
- Extraia regras de negócio e integração com API para `services` e `hooks` quando fizer sentido.
- Evite acoplamento excessivo entre páginas e detalhes de implementação.

## 📌 Comandos Úteis

| Comando | Finalidade |
| --- | --- |
| `docker compose up --build` | Sobe a aplicação com rebuild da imagem |
| `npm run dev` | Executa o frontend em modo desenvolvimento fora do container |
| `npm run build` | Gera o build de produção |
| `npm run lint` | Executa as regras de ESLint |

## 🌐 Visão Geral do Projeto

O Atlas - Frontend Service é a camada de apresentação do ecossistema Atlas. Seu objetivo é entregar uma interface consistente para operar o sistema de bolsas de estudo, consumindo os serviços do backend por meio de uma arquitetura modular, escalável e fácil de manter.

---

> Dica: ao evoluir o projeto, mantenha a separação entre apresentação, regra de negócio e integração com API. Isso reduz retrabalho e facilita a escala da base de código.
