# Desafio QA Beedoo 2026

## Objetivo da Aplicação

A aplicação é uma plataforma de gerenciamento de cursos que permite cadastrar, listar e excluir cursos. O módulo principal analisado é o de **Cadastro de Cursos**.

---

## Principais Fluxos Disponíveis

- **Cadastro de curso:** Formulário com os campos Nome do curso, Descrição, Instrutor, URL da imagem de capa, Data de início, Data de fim, Número de vagas e Tipo de curso.
- **Listagem de cursos:** Visualização dos cursos cadastrados.
- **Exclusão de curso:** Botão para excluir um curso existente.

---

## Pontos Críticos para Teste

- Validação dos campos obrigatórios no cadastro de curso
- Comportamento com dados inválidos (datas incorretas, campos vazios, valores negativos em vagas)
- Fluxo completo de cadastro com todos os campos preenchidos corretamente
- Exclusão de curso (endpoint DELETE)
- Ausência de endpoint para edição de curso (PUT/PATCH)
- Ausência de endpoint para visualização de detalhes de um curso por ID (GET /courses/{id})

---

## Cenários de Teste

### CT-001 - Cadastro de curso com todos os campos preenchidos corretamente

**Pré-condição:** Usuário está na tela de cadastro de curso.

| Campo            | Valor utilizado              |
|------------------|------------------------------|
| Nome do curso    | Curso de QA Avançado         |
| Descrição        | Aprenda QA do zero ao avançado |
| Instrutor        | João Silva                   |
| URL de capa      | https://img.exemplo.com/capa.jpg |
| Data de início   | 01/06/2026                   |
| Data de fim      | 30/06/2026                   |
| Número de vagas  | 50                           |
| Tipo de curso    | Online                       |

**Resultado esperado:** Curso cadastrado com sucesso e exibido na listagem.
**Resultado obtido:** Curso cadastrado com sucesso.
**Status:** Passou

---

### CT-002 - Cadastro de curso com campos obrigatórios vazios

**Pré-condição:** Usuário está na tela de cadastro de curso.

| Ação                        | Detalhe                          |
|-----------------------------|----------------------------------|
| Deixar todos os campos em branco | Não preencher nenhum campo  |
| Clicar em "Cadastrar Curso" | —                                |

**Resultado esperado:** Sistema exibe mensagem de erro para cada campo obrigatório e bloqueia o cadastro.
**Resultado obtido:** Curso é cadastrado com sucesso mesmo sem nenhum dado preenchido.
**Status:** Falhou — ver **BUG-005**

---

### CT-003 - Cadastro de curso com data de fim anterior à data de início

**Pré-condição:** Usuário está na tela de cadastro de curso.

| Campo           | Valor utilizado |
|-----------------|-----------------|
| Data de início  | 10/06/2026      |
| Data de fim     | 01/01/2026      |

**Resultado esperado:** Sistema valida e bloqueia o cadastro com mensagem de erro informando que a data de fim não pode ser anterior à data de início.
**Resultado obtido:** Curso é cadastrado com sucesso mesmo com data de fim inválida.
**Status:** Falhou — ver **BUG-007**

---

### CT-004 - Cadastro de curso com número de vagas negativo

**Pré-condição:** Usuário está na tela de cadastro de curso.

| Campo            | Valor utilizado |
|------------------|-----------------|
| Número de vagas  | -1111118        |

**Resultado esperado:** Sistema valida e bloqueia o cadastro, exibindo mensagem que o número de vagas deve ser maior que zero.
**Resultado obtido:** Curso é cadastrado com sucesso com número de vagas negativo.
**Status:** Falhou — ver **BUG-008**

---

### CT-005 - Cadastro de curso com URL de imagem de capa inválida

**Pré-condição:** Usuário está na tela de cadastro de curso.

| Campo              | Valor utilizado      |
|--------------------|----------------------|
| URL da imagem de capa | abc123 (texto livre) |

**Resultado esperado:** Sistema valida e bloqueia o cadastro informando que a URL deve ser válida (ex: `https://...`).
**Resultado obtido:** Curso é cadastrado com sucesso com valor inválido no campo de URL.
**Status:** Falhou — ver **BUG-009**

---

### CT-006 - Cadastro de curso online com link de inscrição inválido

**Pré-condição:** Usuário está na tela de cadastro de curso com tipo "Online" selecionado.

| Campo              | Valor utilizado        |
|--------------------|------------------------|
| Tipo de curso      | Online                 |
| Link de inscrição  | não é uma url          |

**Resultado esperado:** Sistema valida e bloqueia o cadastro informando que o link de inscrição deve ser uma URL válida.
**Resultado obtido:** Curso é cadastrado com sucesso mesmo com link inválido.
**Status:** Falhou — ver **BUG-011**

---

### CT-007 - Listagem de cursos sem nenhum curso cadastrado

**Pré-condição:** Não há cursos cadastrados na aplicação.

| Ação                        | Detalhe                         |
|-----------------------------|---------------------------------|
| Acessar a listagem de cursos | Nenhum curso cadastrado        |

**Resultado esperado:** Tela exibe mensagem informativa como "Nenhum curso cadastrado no momento".
**Resultado obtido:** Tela exibida em branco, sem nenhuma mensagem.
**Status:** Falhou — ver **BUG-006**

---

### CT-008 - Verificar formatação das datas na listagem de cursos

**Pré-condição:** Ao menos um curso cadastrado com datas de início e fim.

| Campo verificado  | Formato esperado | Formato obtido |
|-------------------|------------------|----------------|
| Data de início    | 18/03/2026       | 2026-03-18     |
| Data de fim       | 30/06/2026       | 2026-06-30     |

**Resultado esperado:** Datas exibidas no formato `dd/mm/aaaa`.
**Resultado obtido:** Datas exibidas no formato bruto ISO `YYYY-MM-DD`.
**Status:** Falhou — ver **BUG-010**

---

### CT-009 - Exclusão de curso existente

**Pré-condição:** Ao menos um curso cadastrado e visível na listagem.

| Ação                              | Detalhe                        |
|-----------------------------------|--------------------------------|
| Clicar no botão "Excluir Curso"   | Curso existente na listagem    |

**Resultado esperado:** Curso excluído com sucesso (HTTP 200 ou 204) e removido da listagem.
**Resultado obtido:** HTTP 405 Method Not Allowed.
**Status:** Falhou — ver **BUG-001**

---

### CT-010 - Clicar no logo "Beedoo QA Challenge" no header

**Pré-condição:** Usuário está em qualquer página da aplicação.

| Ação                                          | Detalhe                        |
|-----------------------------------------------|--------------------------------|
| Clicar no letreiro "Beedoo QA Challenge"      | Canto esquerdo do header       |

**Resultado esperado:** Usuário é redirecionado para a tela de listagem de cursos.
**Resultado obtido:** Nenhuma ação ocorre ao clicar no logo.
**Status:** Falhou — ver **BUG-004**

---

### CT-011 - Edição de curso existente via API (PUT/PATCH)

**Pré-condição:** Ao menos um curso cadastrado.

| Ação                                              | Detalhe                                                      |
|---------------------------------------------------|--------------------------------------------------------------|
| Realizar requisição `PUT` ou `PATCH` para `/test-api/courses/{id}` | Com payload de atualização do curso |

**Resultado esperado:** Curso atualizado com sucesso (HTTP 200).
**Resultado obtido:** Endpoint inexistente / não implementado.
**Status:** Falhou — ver **BUG-002**

---

### CT-012 - Buscar detalhes de um curso por ID via API (GET)

**Pré-condição:** Ao menos um curso cadastrado com ID conhecido.

| Ação                                              | Detalhe                           |
|---------------------------------------------------|-----------------------------------|
| Realizar requisição `GET` para `/test-api/courses/{id}` | Com ID de curso existente   |

**Resultado esperado:** Retorno dos detalhes completos do curso (HTTP 200).
**Resultado obtido:** Endpoint inexistente / não implementado.
**Status:** Falhou — ver **BUG-003**

---

### CT-013 - Persistência dos dados cadastrados na API

**Pré-condição:** Usuário cadastra um ou mais cursos.

| Ação                                          | Detalhe                                              |
|-----------------------------------------------|------------------------------------------------------|
| Cadastrar curso e inspecionar o localStorage  | Verificar se os dados são enviados ao endpoint POST  |

**Resultado esperado:** Dados enviados e persistidos na API (`POST /test-api/courses`), disponíveis mesmo após limpar o navegador.
**Resultado obtido:** Dados salvos apenas no localStorage do navegador, sem envio real à API.
**Status:** Falhou — ver **BUG-012**

---

## Bugs Encontrados

### BUG-005 - Cadastro de curso aceita envio com campos vazios e sem validação de formato

| Campo              | Descrição                                              |
|--------------------|--------------------------------------------------------|
| **Título**         | Sistema permite cadastrar curso sem preencher nenhum campo e sem validar formato dos dados |
| **Passos para reproduzir** | 1. Acessar a tela de cadastro de curso <br> 2. Deixar todos os campos em branco <br> 3. Clicar em "Cadastrar Curso" |
| **Resultado atual**  | Curso é cadastrado com sucesso mesmo sem nenhum dado preenchido |
| **Resultado esperado** | Sistema deve validar campos obrigatórios (Nome, Descrição, Instrutor, Datas, Vagas, Tipo) e o formato correto de cada campo antes de permitir o cadastro |
| **Endpoint**       | `POST https://creative-sherbet-a51eac.netlify.app/test-api/courses` |
| **Severidade**     | Alta                                                   |

---

### BUG-012 - Dados do cadastro de curso são salvos apenas no localStorage e não persistidos na API

| Campo              | Descrição                                              |
|--------------------|--------------------------------------------------------|
| **Título**         | Sistema armazena os cursos somente no localStorage do navegador, sem enviar para a API, causando perda de dados |
| **Passos para reproduzir** | 1. Cadastrar um ou mais cursos <br> 2. Inspecionar o localStorage do navegador <br> 3. Verificar que os dados existem apenas localmente e não são enviados ao endpoint `POST /test-api/courses` |
| **Resultado atual**  | Cursos salvos apenas no localStorage, com dados inválidos e sem persistência real. Exemplo do payload armazenado: <br><br> `[{"id":1,"name":"","description":"","cover":"","startDate":"2026-05-14","endDate":"2026-03-12","numberOfVagas":"","type":{"label":"","value":""},"address":"","instructor":"","url":""},{"id":2,"name":"","description":"","cover":"osanopiksmopmsps","startDate":"2026-03-18","endDate":"2026-03-02","numberOfVagas":"-111111817718","type":{"label":"","value":""},"address":"","instructor":"  ","url":""},{"id":3,"name":"","description":"","cover":"","startDate":"","endDate":"","numberOfVagas":"","type":{"label":"Online","value":"online"},"address":"0o","instructor":"","url":"dddddd"}]` |
| **Resultado esperado** | Dados devem ser enviados e persistidos na API (`POST https://creative-sherbet-a51eac.netlify.app/test-api/courses`), garantindo que não sejam perdidos ao limpar o navegador ou trocar de dispositivo |
| **Endpoint**       | `POST https://creative-sherbet-a51eac.netlify.app/test-api/courses` |
| **Severidade**     | Crítica                                                |

---

### BUG-011 - Campo "Link de inscrição" não valida se o valor é uma URL válida

| Campo              | Descrição                                              |
|--------------------|--------------------------------------------------------|
| **Título**         | Sistema permite cadastrar curso online com valor inválido no campo de link de inscrição |
| **Passos para reproduzir** | 1. Acessar a tela de cadastro de curso <br> 2. Selecionar o tipo "Online" <br> 3. Preencher o campo "Link de inscrição" com um texto qualquer (ex: `abc123`, `não é uma url`) <br> 4. Clicar em "Cadastrar Curso" |
| **Resultado atual**  | Curso é cadastrado com sucesso mesmo com valor inválido no campo de link de inscrição |
| **Resultado esperado** | Sistema deve validar que o valor informado é uma URL válida (ex: `https://...`) antes de permitir o cadastro |
| **Severidade**     | Média                                                  |

---

### BUG-010 - Datas exibidas na listagem de cursos sem formatação adequada

| Campo              | Descrição                                              |
|--------------------|--------------------------------------------------------|
| **Título**         | Datas exibidas na listagem de cursos no formato bruto do JavaScript (`YYYY-MM-DD`) em vez de formato legível |
| **Passos para reproduzir** | 1. Cadastrar um curso com datas de início e fim <br> 2. Acessar a listagem de cursos e observar as datas exibidas |
| **Resultado atual**  | Datas exibidas no formato `2026-03-18` (formato ISO/input Date do JavaScript) |
| **Resultado esperado** | Datas devem ser formatadas para exibição amigável, ex: `18/03/2026` |
| **Severidade**     | Baixa                                                  |

---

### BUG-009 - Campo "URL da imagem de capa" aceita valores que não são URLs válidas

| Campo              | Descrição                                              |
|--------------------|--------------------------------------------------------|
| **Título**         | Sistema permite cadastrar curso com valor inválido no campo de URL da imagem de capa |
| **Passos para reproduzir** | 1. Acessar a tela de cadastro de curso <br> 2. Preencher o campo "URL da imagem de capa" com um texto qualquer (ex: `abc123`, `não é uma url`) <br> 3. Clicar em "Cadastrar Curso" |
| **Resultado atual**  | Curso é cadastrado com sucesso mesmo com valor inválido no campo de URL |
| **Resultado esperado** | Sistema deve validar que o valor informado é uma URL válida (ex: `https://...`) antes de permitir o cadastro |
| **Severidade**     | Média                                                  |

---

### BUG-008 - Campo "Número de vagas" aceita valores negativos

| Campo              | Descrição                                              |
|--------------------|--------------------------------------------------------|
| **Título**         | Sistema permite cadastrar curso com número de vagas negativo |
| **Passos para reproduzir** | 1. Acessar a tela de cadastro de curso <br> 2. Preencher o campo "Número de vagas" com um valor negativo (ex: -1111118) <br> 3. Clicar em "Cadastrar Curso" |
| **Resultado atual**  | Curso é cadastrado com sucesso com número de vagas negativo |
| **Resultado esperado** | Sistema deve validar que o número de vagas é um valor inteiro maior que zero |
| **Severidade**     | Alta                                                   |

---

### BUG-007 - Data de fim aceita valor menor que a data de início

| Campo              | Descrição                                              |
|--------------------|--------------------------------------------------------|
| **Título**         | Sistema permite cadastrar curso com data de fim anterior à data de início |
| **Passos para reproduzir** | 1. Acessar a tela de cadastro de curso <br> 2. Preencher a data de início com uma data futura (ex: 10/06/2026) <br> 3. Preencher a data de fim com uma data anterior (ex: 01/01/2026) <br> 4. Clicar em "Cadastrar Curso" |
| **Resultado atual**  | Curso é cadastrado com sucesso mesmo com a data de fim menor que a data de início |
| **Resultado esperado** | Sistema deve validar que a data de fim é igual ou posterior à data de início e bloquear o cadastro caso contrário |
| **Severidade**     | Alta                                                   |

---

### BUG-006 - Listagem de cursos não exibe mensagem quando não há cursos cadastrados

| Campo              | Descrição                                              |
|--------------------|--------------------------------------------------------|
| **Título**         | Tela de listagem de cursos não exibe mensagem de estado vazio |
| **Passos para reproduzir** | 1. Acessar a listagem de cursos sem nenhum curso cadastrado |
| **Resultado atual**  | Tela exibida em branco, sem nenhuma mensagem informativa |
| **Resultado esperado** | Deve exibir uma mensagem como "Nenhum curso cadastrado no momento" |
| **Severidade**     | Baixa                                                  |

---

### BUG-002 - Ausência de endpoint para edição de curso

| Campo              | Descrição                                              |
|--------------------|--------------------------------------------------------|
| **Título**         | Sistema não possui endpoint para editar curso          |
| **Passos para reproduzir** | 1. Tentar realizar uma requisição `PUT` ou `PATCH` em `https://creative-sherbet-a51eac.netlify.app/test-api/courses/{id}` |
| **Resultado atual**  | Endpoint inexistente / não implementado              |
| **Resultado esperado** | Deve ser possível editar os dados de um curso existente |
| **Severidade**     | Alta                                                   |

---

### BUG-003 - Ausência de endpoint para detalhes de curso por ID

| Campo              | Descrição                                              |
|--------------------|--------------------------------------------------------|
| **Título**         | Sistema não possui endpoint para buscar detalhes de um curso por ID |
| **Passos para reproduzir** | 1. Tentar realizar uma requisição `GET` em `https://creative-sherbet-a51eac.netlify.app/test-api/courses/{id}` |
| **Resultado atual**  | Endpoint inexistente / não implementado              |
| **Resultado esperado** | Deve ser possível visualizar todos os detalhes de um curso específico pelo seu ID |
| **Severidade**     | Média                                                  |

---

### BUG-004 - Logo "Beedoo QA Challenge" no header não redireciona para a listagem de cursos

| Campo              | Descrição                                              |
|--------------------|--------------------------------------------------------|
| **Título**         | Clique no logo "Beedoo QA Challenge" no header não redireciona para a listagem de cursos |
| **Passos para reproduzir** | 1. Acessar qualquer página da aplicação <br> 2. Clicar no letreiro "Beedoo QA Challenge" no canto esquerdo do header |
| **Resultado atual**  | Nenhuma ação ocorre ao clicar no logo                |
| **Resultado esperado** | Deve redirecionar o usuário para a tela de listagem de cursos |
| **Severidade**     | Baixa                                                  |

---

### BUG-001 - Botão "Excluir Curso" retorna 405 Method Not Allowed

| Campo              | Descrição                                      |
|--------------------|------------------------------------------------|
| **Título**         | Botão "Excluir Curso" retorna 405 Method Not Allowed |
| **Passos para reproduzir** | 1. Acessar a listagem de cursos <br> 2. Clicar no botão de excluir em um curso existente |
| **Resultado atual**  | HTTP 405 Method Not Allowed                  |
| **Resultado esperado** | Curso excluído com sucesso (HTTP 200 ou 204) |
| **Endpoint**       | `DELETE https://creative-sherbet-a51eac.netlify.app/test-api/courses/{id}` |
| **Severidade**     | Alta                                           |


