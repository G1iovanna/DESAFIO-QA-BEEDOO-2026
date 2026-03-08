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


