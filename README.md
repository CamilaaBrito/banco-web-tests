# Banco Web Tests 

Projeto de testes automatizados desenvolvido na **Mentoria 2.0 do Julio de Lima** para demonstrar as melhores práticas de automação de testes com Cypress e JavaScript.

## Objetivo

Este projeto é um exercício prático que tem como objetivo ensinar aos alunos da mentoria 2.0 do julia de lima como automatizar testes end-to-end (E2E) utilizando o framework Cypress, demonstrando:

- Organização de testes com Cypress
- Criação de custom commands reutilizáveis
- Uso de fixtures para dados de teste
- Geração de relatórios com MochaAwesome Reporter
- Boas práticas de automação de testes

## Componentes do Projeto

### Estrutura de Diretórios

```
banco-web-tests/
├── cypress/
│   ├── e2e/                    # Testes end-to-end
│   │   ├── login.cy.js         # Testes de login
│   │   └── transferencia.cy.js # Testes de transferências
│   ├── fixtures/               # Dados de teste
│   │   ├── credenciais.json    # Credenciais para login
│   │   └── example.json        # Exemplo de fixture
│   ├── support/                # Configurações e commands
│   │   ├── e2e.js              # Setup dos testes
│   │   ├── commands.js         # Carregamento de custom commands
│   │   └── commands/
│   │       ├── common.js       # Commands comuns
│   │       ├── login.js        # Commands de login
│   │       └── transferencia.js# Commands de transferência
│   └── videos/                 # Vídeos dos testes
├── cypress.config.js           # Configuração do Cypress
├── package.json                # Dependências do projeto
└── README.md                   # Documentação do projeto
```

### Componentes Principais

#### 1. **Testes (cypress/e2e/)**

- **login.cy.js**: Testes relacionados ao login na aplicação
- **transferencia.cy.js**: Testes das funcionalidades de transferência bancária

#### 2. **Custom Commands (cypress/support/commands/)**

Funções reutilizáveis que encapsulam ações comuns:

- **common.js**: Comandos genéricos
- **login.js**: Comandos específicos de login
- **transferencia.js**: Comandos para transferências

#### 3. **Fixtures (cypress/fixtures/)**

Dados de teste armazenados em arquivos JSON:

- **credenciais.json**: Credenciais válidas e inválidas para testes de login
- **example.json**: Exemplo de fixture

#### 4. **Relatórios**

- Gerados automaticamente com **cypress-mochawesome-reporter**

## Pré-requisitos

Antes de começar, certifique-se de ter instalado:

- **Node.js** (v14 ou superior) - [Baixar aqui](https://nodejs.org/)
- **npm** (geralmente incluído com Node.js)
- **Git** (opcional) - [Baixar aqui](https://git-scm.com/)

### Aplicações Necessárias

Este projeto depende de duas aplicações externas que devem estar rodando localmente:

1. **Banco API** - Backend da aplicação
   - Repository: https://github.com/juliodelimas/banco-api
   - Deve rodar em uma porta específica (verifique a configuração)

2. **Banco Web** - Frontend da aplicação
   - Repository: https://github.com/juliodelimas/banco-web
   - URL padrão: `http://localhost:4000`

## 📦 Instalação

### Passo 1: Clonar o Repositório

```bash
git clone https://github.com/CamilaaBrito/banco-web-tests.git
cd banco-web-tests
```

### Passo 2: Instalar Dependências

```bash
npm install
```

Este comando irá instalar:
- npm install --save-dev cypress (v14.5.1) - Framework de testes
- **cypress-mochawesome-reporter** (v4.0.2) - Gerador de relatórios

### Passo 3: Configurar Aplicações Necessárias

1. **Clone e configure o Banco API:**
   ```bash
   git clone https://github.com/juliodelimas/banco-api.git
   cd banco-api
   npm install
   npm run rest-api
   
   ```

2. **Em outro terminal, clone e configure o Banco Web:**
   ```bash
   git clone https://github.com/juliodelimas/banco-web.git
   cd banco-web
   npm install
   npm run server
   ```

3. **Verifique se as aplicações estão rodando:**
   - Banco Web: http://localhost:4000
   - Banco API: (verificar porta conforme configuração)

## ▶️ Executando os Testes

### Scripts Disponíveis

No arquivo `package.json` estão configurados os seguintes scripts:

#### 1. **Executar testes em modo headless (sem interface gráfica)**
```bash
npm test
```
Ou manualmente:
```bash
npx cypress run
```

#### 2. **Executar testes com interface gráfica**
```bash
npm run cy:headed
```
```bash
npx cypress run --headed
```

#### 3. **Abrir o Cypress Test Runner (modo interativo)**
```bash
npm run cy:open
```
```bash
npx cypress open
```

### Visualizar Relatórios

Após executar os testes, abra o relatório HTML:
```
cypress/reports/html/index.html
```

## Documentação dos Testes

### Suite: Login

**Arquivo:** `cypress/e2e/login.cy.js`

#### Teste 1: Login com dados válidos

**Objetivo:** Validar que um usuário com credenciais corretas consegue fazer login na aplicação.

#### Teste 2: Login com dados inválidos

**Objetivo:** Validar que a aplicação exibe uma mensagem de erro ao tentar fazer login com credenciais incorretas.

### Suite: Transferências

**Arquivo:** `cypress/e2e/transferencia.cy.js`

#### Teste 1: Transferência com valor válido

**Objetivo:** Validar que uma transferência com valor dentro dos limites é realizada com sucesso.

#### Teste 2: Transferência acima do limite sem autenticação

**Objetivo:** Validar que a aplicação solicita autenticação adicional para transferências acima de R$5.000,00.

## 🛠️ Custom Commands

Os custom commands são funções reutilizáveis que encapsulam ações repetitivas nos testes. Eles estão organizados em arquivos específicos dentro de `cypress/support/commands/`.

### Commands Comuns (common.js)

#### `cy.verificarMensagemNoToast(mensagem)`
Verifica se uma mensagem é exibida no componente toast (notificação).

**Parâmetros:**
- `mensagem` (string): A mensagem esperada no toast

**Exemplo:**
```javascript
cy.verificarMensagemNoToast('Transferência realizada!')
```

**O que faz:**
- Verifica se o elemento `.toast` contém o texto esperado

---

#### `cy.selecionarOpcaoNaCombobox(labelDoCampo, opcao)`
Seleciona uma opção em um campo de combobox/select.

**Parâmetros:**
- `labelDoCampo` (string): ID ou atributo `for` do label do campo
- `opcao` (string): Texto da opção a selecionar

**Exemplo:**
```javascript
cy.selecionarOpcaoNaCombobox('conta-origem', 'Maria Oliveira')
```

**O que faz:**
- Localiza o campo pela label
- Clica no campo para abrir o dropdown
- Seleciona a opção desejada

---

### Commands de Login (login.js)

#### `cy.fazerLoginComCredenciaisValidas()`
Realiza login utilizando credenciais válidas armazenadas em `credenciais.json`.

**Exemplo:**
```javascript
cy.fazerLoginComCredenciaisValidas()
```

**O que faz:**
- Carrega o arquivo de credenciais
- Preenche o campo de usuário
- Preenche o campo de senha
- Clica no botão de login

**Credenciais utilizadas:**
```json
{
  "usuario": "julio.lima",
  "senha": "123456"
}
```

---

#### `cy.fazerLoginComCredenciaisInvalidas()`
Realiza login utilizando credenciais inválidas para testar tratamento de erros.

**Exemplo:**
```javascript
cy.fazerLoginComCredenciaisInvalidas()
```

**O que faz:**
- Carrega o arquivo de credenciais
- Preenche o campo de usuário com credencial inválida
- Preenche o campo de senha com credencial inválida
- Clica no botão de login (Entrar)

**Credenciais utilizadas:**
```json
{
  "usuario": "julio.lima",
  "senha": "123457"
}
```

---

### Commands de Transferência (transferencia.js)

#### `cy.realizarTransferencia(contaOrigem, contaDestino, valor)`
Realiza uma transferência bancária entre duas contas.

**Parâmetros:**
- `contaOrigem` (string): Nome da conta de origem
- `contaDestino` (string): Nome da conta de destino
- `valor` (number): Valor a transferir

**Exemplo:**
```javascript
cy.realizarTransferencia('Maria Oliveira', 'João da Silva', 100)
```

**O que faz:**
- Seleciona a conta de origem no combobox
- Seleciona a conta de destino no combobox
- Preenche o campo de valor
- Clica no botão Transferir

---

## 📊 Estrutura de Dados

### Credenciais (fixtures/credenciais.json)

```json
{
  "valida": {
    "usuario": "julio.lima",
    "senha": "123456"
  },
  "invalida": {
    "usuario": "julio.lima",
    "senha": "123457"
  }
}

### Configurações Principais

- **baseUrl**: `http://localhost:4000` - URL onde a aplicação web está rodando
- **reporter**: Configurado para usar o `cypress-mochawesome-reporter`
- **setupNodeEvents**: Inicializa o plugin do reporter

## 📝 Padrões de Escrita de Testes

Este projeto segue algumas convenções para manter o código organizado:

### Nomes de Testes
- Utilizam linguagem clara em português
- Descrevem o comportamento esperado

### Organização
- Um custom command por ação específica
- Testes organizados por funcionalidade (describe blocks)
- Fixtures em JSON para dados reutilizáveis

### Boas Práticas
- Usar `beforeEach` para setup comum
- Usar custom commands para ações repetitivas
- Manter testes independentes e isolados
- Usar fixtures para dados de teste
- Adicionar screenshots para documentação visual

## Troubleshooting

### Aplicação não está acessível em localhost:4000

**Solução:**
- Verifique se o Banco Web está rodando
- Verifique se a porta 4000 está disponível
- Altere a `baseUrl` em `cypress.config.js` se necessário

## Links Úteis

- [Documentação Cypress](https://docs.cypress.io/)
- [Documentação MochaAwesome Reporter](https://www.npmjs.com/package/cypress-mochawesome-reporter)
- [Banco API Repository](https://github.com/juliodelimas/banco-api)
- [Banco Web Repository](https://github.com/juliodelimas/banco-web)

## Recursos Adicionais

### Melhorias Futuras

- Adicionar testes de segurança
- Implementar Page Object Model (POM)
- Adicionar testes de performance
- Integrar com CI/CD (GitHub Actions, Jenkins)
- Adicionar testes de acessibilidade

## Autor

Desenvolvido como parte da **Mentoria 2.0 do Julio de Lima**

---

**Última atualização:** Abril de 2026
