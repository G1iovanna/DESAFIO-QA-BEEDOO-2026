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

### CT-001 - Cadastro de curso com todos os campos preenchidos

| Campo            | Valor utilizado        |
|------------------|------------------------|
| Nome do curso    | Informado              |
| Descrição        | Informada              |
| Instrutor        | Informado              |
| URL de capa      | Informada              |
| Data de início   | Informada (dd/mm/aaaa) |
| Data de fim      | Informada (dd/mm/aaaa) |
| Número de vagas  | Informado              |
| Tipo de curso    | Selecionado            |

**Resultado esperado:** Curso cadastrado com sucesso.
**Resultado obtido:** Curso cadastrado com sucesso.
**Status:** Passou

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


