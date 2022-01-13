<h1 align="center">
Json Server Marta - Fake API
</h1>

<p align = "center">
Este é um exercício de criação de uma fake API que armazena usuários, seus respectivos cursos e hobbies.
</p>

<p align="center">
  <a href="#endpoints">Endpoints</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</p>

## **Endpoints**

A API tem um total de 7 endpoints.

Destes, 3 podem ser usados para cadastrar o usuário:

POST /register <br/>
POST /signup <br/>
POST /users <br/>

Dois podem ser usados para efetuar o login:

POST /login <br/>
POST /signin<br/>

Um para o usuário cadastrar e acessar seus cursos:

POST /courses <br/>
GET /courses <br/>
(obs: todos os usuários podem acessar os cursos registrados, mas apenas o usuário logado pode cadastrar um curso)

E um para o usuário cadastrar e acessar seus hobbies:

POST /hobbies <br/>
GET /hobbies <br/>
(obs: apenas usuários logados podem acessar os hobbies)

O url base da API é https://json-server-marta.herokuapp.com

## Rotas que não precisam de autenticação

<h2 align ='center'> Criação de usuário </h2>

`POST /register - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "marta@mail.com",
  "password": "123456",
  "username": "marta",
  "age": "37"
}
```

OBS: apenas os campos email e password são obrigatórios.

Caso dê tudo certo, a resposta será assim:

`POST /register - FORMATO DA RESPOSTA - STATUS 201`

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Im1hcnRhQG1haWwuY29tIiwiaWF0IjoxNjQyMDk4ODA3LCJleHAiOjE2NDIxMDI0MDcsInN1YiI6IjQifQ.wDksEHIy2p8GYRwTCfhO7zfx75UKcs9tHlfCEs1WJ5M",
  "user": {
    "email": "marta@mail.com",
    "username": "marta",
    "age": "37",
    "id": 4
  }
}
```

<h2 align ='center'> Possíveis erros </h2>

Caso algum campo vá em branco, dará o seguinte erro.

`POST /users - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
{
  "status": "error",
  "message": "Email and password are required"
}
```

Email já cadastrado:

`POST /users - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
{
  "status": "error",
  "message": "Email already exists"
}
```

<h2 align = "center"> Login </h2>

`POST /login - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "marta@mail.com",
  "password": "123456"
}
```

Caso dê tudo certo, a resposta será assim:

`POST /login - FORMATO DA RESPOSTA - STATUS 201`

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Im1hcnRhQG1haWwuY29tIiwiaWF0IjoxNjQyMDk5NTk4LCJleHAiOjE2NDIxMDMxOTgsInN1YiI6IjQifQ.k1nN6h_8oezQkk7liIVeeU7b8_KsJmVej6TdjNwdQEI",
  "user": {
    "email": "marta@mail.com",
    "username": "marta",
    "age": "37",
    "id": 4
  }
}
```

<p>Nessa resposta temos o token de autenticação para as rotas em que ele é necessário e também o id do usuário, que será solicitado para criação de cursos e hobbies.</p>

<h2 align ='center'> Buscar todos os cursos cadastrados </h2>

Requisição não necessita de body.

`GET /courses - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "language": "react",
    "level": "intermediate",
    "userId": "2",
    "id": 1
  },
  {
    "language": "javascript",
    "level": "advanced",
    "userId": "2",
    "id": 2
  },
  {
    "language": "python",
    "level": "beginner",
    "userId": "2",
    "id": 3
  }
]
```

## Rotas que necessitam de autorização

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {token}

<p>Após o usuário estar logado, ele deve conseguir criar novos cursos, hobbies e acessar toda a lista de hobbies.</p>

<p>Também poderá acessar suas própias informações passando o id como parâmetro.</p>

<h2 align ='center'> Acessar seu pŕoprio perfil </h2>

`GET /users/user_id - FORMATO DA RESPOSTA - STATUS 200`

```json
{
  "email": "kenzinha@mail.com",
  "password": "$2a$10$YQiiz0ANVwIgpOjYXPxc0O9H2XeX3m8OoY1xk7OGgxTnOJnsZU7FO",
  "name": "Kenzinha",
  "age": 25,
  "id": 2
}
```

<h2 align ='center'> Criar cursos para o seu perfil </h2>

`POST /courses - FORMATO DA REQUISIÇÃO`

```json
{ "language": "react", "level": "intermediate", "userId": "2" }
```

<h2 align ='center'> Criar hobbies para o seu perfil </h2>

`POST /hobbies - FORMATO DA REQUISIÇÃO`

```json
{ "category": "health", "title": "running", "userId": "2" }
```

<h2 align ='center'> Buscar todos os hobbies cadastrados </h2>

Requisição não necessita de body

`GET /hobbies - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "category": "health",
    "title": "running",
    "userId": "2",
    "id": 1
  },
  {
    "category": "culture",
    "title": "reading",
    "userId": "2",
    "id": 2
  }
]
```
