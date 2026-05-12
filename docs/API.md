# API do Yearbook — Documentação de Endpoints

    Base URL (produção): `https://yearbook-backend.vercel.app`

## Convenções

    - Todas as respostas são em JSON
    - Rotas protegidas exigem header `Authorization: Bearer <token>`
    - O campo `senhaHash` nunca é retornado em nenhuma resposta
    - Erros seguem o formato `{ "erro": "mensagem descritiva" }`

## Auth

### POST /auth/register

    Cria uma nova conta de aluno.

- **Autenticação:** Não
 - **Body:**

    ```json
    {
      "nome": "Maria Silva",
      "email": "maria@email.com",
      "senha": "minhasenha123",
      "cidade": "Salinas",
      "frase": "Aqui começa o futuro.",
      "planosFuturos": "Cursar Ciência da Computação na UFMG"
    }
    ```

- **Resposta de sucesso:** `201 Created`

    ```json
    {
      "id": 1,
      "nome": "Maria Silva",
      "email": "maria@email.com",
      "cidade": "Salinas",
      "frase": "Aqui começa o futuro.",
      "planosFuturos": "Cursar Ciência da Computação na UFMG",
      "fotoUrl": null,
      "role": "USER",
      "criadoEm": "2026-04-03T10:30:00.000Z"
    }
    ```

- **Erros:**
      - `400` — Campos obrigatórios ausentes
      - `409` — Email já cadastrado
## Alunos

### GET /alunos
Lista todos os alunos.

```json
{
  "Autenticacao": "Não",
  "Body": "Nenhum",
  "RespostaDeSucesso": [
    {
      "id": 1,
      "nome": "Maria Silva",
      "email": "maria@email.com",
      "cidade": "Salinas",
      "frase": "Aqui começa o futuro.",
      "planosFuturos": "Cursar Ciência da Computação na UFMG",
      "fotoUrl": null,
      "role": "USER",
      "criadoEm": "2026-04-03T10:30:00.000Z"
    },
    {
      "id": 2,
      "nome": "João Pereira",
      "email": "joao@email.com",
      "cidade": "Belo Horizonte",
      "frase": "Aprender sempre.",
      "planosFuturos": "Trabalhar em tecnologia",
      "fotoUrl": null,
      "role": "USER",
      "criadoEm": "2026-04-03T11:00:00.000Z"
    }
  ],
  "Erros": "Nenhum específico"
}

### GET /alunos/:id
Busca um aluno pelo ID.

```json
{
  "Autenticacao": "Não",
  "Body": "Nenhum",
  "RespostaDeSucesso": {
    "id": 1,
    "nome": "Maria Silva",
    "email": "maria@email.com",
    "cidade": "Salinas",
    "frase": "Aqui começa o futuro.",
    "planosFuturos": "Cursar Ciência da Computação na UFMG",
    "fotoUrl": null,
    "role": "USER",
    "criadoEm": "2026-04-03T10:30:00.000Z"
  },
  "Erros": {
    "404": "Aluno não encontrado"
  }
}

### PUT /alunos/:id
Atualiza o próprio perfil.

```json
{
  "Autenticacao": "Bearer token",
  "Body": {
    "nome": "Maria Silva Atualizada",
    "cidade": "Belo Horizonte",
    "frase": "Seguindo novos caminhos",
    "planosFuturos": "Cursar Engenharia",
    "fotoUrl": "https://url-da-foto.com/maria.jpg"
  },
  "RespostaDeSucesso": {
    "id": 1,
    "nome": "Maria Silva Atualizada",
    "email": "maria@email.com",
    "cidade": "Belo Horizonte",
    "frase": "Seguindo novos caminhos",
    "planosFuturos": "Cursar Engenharia",
    "fotoUrl": "https://url-da-foto.com/maria.jpg",
    "role": "USER",
    "criadoEm": "2026-04-03T10:30:00.000Z"
  },
  "Erros": {
    "401": "Não autenticado",
    "403": "Tentativa de atualizar outro usuário"
  }
}

### DELETE /alunos/:id
Remove um aluno.

```json
{
  "Autenticacao": "Bearer token (admin)",
  "Body": "Nenhum",
  "RespostaDeSucesso": "204 No Content",
  "Erros": {
    "401": "Não autenticado",
    "403": "Usuário não é admin",
    "404": "Aluno não encontrado"
  }
}

## Mensagens

### GET /mensagens
Lista todas as mensagens do mural.

```json
{
  "Autenticacao": "Não",
  "Body": "Nenhum",
  "RespostaDeSucesso": [
    {
      "id": 1,
      "texto": "Boa sorte a todos!",
      "imagemUrl": null,
      "autorId": 1,
      "autor": {
        "id": 1,
        "nome": "Maria Silva",
        "fotoUrl": null
      },
      "criadoEm": "2026-04-03T12:00:00.000Z"
    },
    {
      "id": 2,
      "texto": "Que venha o futuro!",
      "imagemUrl": "https://url-da-imagem.com/foto.jpg",
      "autorId": 2,
      "autor": {
        "id": 2,
        "nome": "João Pereira",
        "fotoUrl": null
      },
      "criadoEm": "2026-04-03T12:30:00.000Z"
    }
  ],
  "Erros": "Nenhum específico"
}

### POST /mensagens
Cria uma nova mensagem.

```json
{
  "Autenticacao": "Bearer token",
  "Body": {
    "texto": "Mensagem de teste",
    "imagemUrl": "https://url-da-imagem.com/foto.jpg"
  },
  "RespostaDeSucesso": {
    "id": 3,
    "texto": "Mensagem de teste",
    "imagemUrl": "https://url-da-imagem.com/foto.jpg",
    "autorId": 1,
    "autor": {
      "id": 1,
      "nome": "Maria Silva",
      "fotoUrl": null
    },
    "criadoEm": "2026-05-12T10:00:00.000Z"
  },
  "Erros": {
    "400": "Campo 'texto' ausente",
    "401": "Não autenticado"
  }
}

### DELETE /mensagens/:id
Exclui uma mensagem.

```json
{
  "Autenticacao": "Bearer token",
  "Body": "Nenhum",
  "RespostaDeSucesso": "204 No Content",
  "Erros": {
    "401": "Não autenticado",
    "403": "Usuário não é dono da mensagem nem admin",
    "404": "Mensagem não encontrada"
  }
}