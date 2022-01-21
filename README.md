<h1 align="center">
Json Server Kenzie Burguer
</h1>

<p align = "center">
Esta é uma fake API que permitem que usuários se cadastrem, façam login, vejam produtos disponíveis e guardem produtos nos seus carrinhos.
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

Um para o usuário acessar os produtos disponíveis:

GET /products <br/>
(obs: qualquer pessoa pode acessar a lista de produtos, mas ninguém está autorizado a adicionar mais produtos além dos que estão na base de dados)

E um para o usuário gerenciar os produtos do seu carrinho:

POST /cart <br/>
GET /cart <br/>
DELETE /cart <br/>
(obs: apenas usuários logados podem acessar ou modificar o carrinho)

O url base da API é https://api-burguer-kenzie.herokuapp.com/

## Rotas que não precisam de autenticação

<h2 align ='center'> Criação de usuário </h2>

`POST /register - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "marta@mail.com",
  "password": "123456",
  "username": "marta"
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
    "id": 4
  }
}
```

<p>Nessa resposta temos o token de autenticação para as rotas em que ele é necessário e também o id do usuário, que será solicitado para criação de cursos e hobbies.</p>

<h2 align ='center'> Buscar os produtos cadastrados </h2>

Requisição não necessita de body.

`GET /products - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "id": 1,
    "name": "Hamburguer",
    "category": "Sanduíches",
    "price": 14,
    "img": "https://i.ibb.co/fpVHnZL/hamburguer.png"
  },
  {
    "id": 2,
    "name": "X-Burguer",
    "category": "Sanduíches",
    "price": 16,
    "img": "https://i.ibb.co/djbw6LV/x-burgue.png"
  },
  {
    "id": 3,
    "name": "Big Kenzie",
    "category": "Sanduíches",
    "price": 18,
    "img": "https://i.ibb.co/FYBKCwn/big-kenzie.png"
  },
  {
    "id": 4,
    "name": "Fanta Guaraná",
    "category": "Bebidas",
    "price": 5,
    "img": "https://i.ibb.co/cCjqmPM/fanta-guarana.png"
  },
  {
    "id": 5,
    "name": "Coca-cola",
    "category": "Bebidas",
    "price": 4.99,
    "img": "https://i.ibb.co/fxCGP7k/coca-cola.png"
  },
  {
    "id": 6,
    "name": "KenzieMaltine",
    "category": "Bebidas",
    "price": 4.99,
    "img": "https://i.ibb.co/QNb3DJJ/milkshake-ovomaltine.png"
  }
]
```

## Rotas que necessitam de autorização

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {token}

<p>Após o usuário estar logado, ele deve conseguir consultar, adicionar e deletar itens do carrinho.</p>

<h2 align ='center'> Adicionar itens ao carrinho </h2>

`POST /cart/ - FORMATO DA REQUISIÇÃO`

```json
{
  "id": 4,
  "name": "Fanta Guaraná",
  "category": "Bebidas",
  "price": 5,
  "img": "https://i.ibb.co/cCjqmPM/fanta-guarana.png"
}
```

<h2 align ='center'> Deletar itens do carrinho </h2>

`DELETE /cart/product_id - FORMATO DA REQUISIÇÃO`

Requisição não necessita de body, apenas o id do produto que deve ser deletado é passado por parâmetro.

<h2 align ='center'> Buscar produtos que estão no carrinho </h2>

Requisição não necessita de body.

`GET /cart - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "id": 4,
    "name": "Fanta Guaraná",
    "category": "Bebidas",
    "price": 5,
    "img": "https://i.ibb.co/cCjqmPM/fanta-guarana.png"
  },
  {
    "id": 1,
    "name": "Hamburguer",
    "category": "Sanduíches",
    "price": 14,
    "img": "https://i.ibb.co/fpVHnZL/hamburguer.png"
  }
]
```
