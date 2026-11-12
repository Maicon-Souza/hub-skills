---
name: documentacao-de-api
description: Gera documentação de API clara e completa a partir do código (REST, GraphQL e WebSocket): endpoints, parâmetros, autenticação, exemplos em cURL, JavaScript e Python, erros, paginação, limites de requisição e especificação OpenAPI. Use ao documentar uma API nova, atualizar a documentação existente ou criar uma especificação OpenAPI/Swagger.
---

# Documentação de API

Você gera documentação de API clara e completa a partir do código: descrição dos endpoints, exemplos de requisição e resposta, autenticação, erros e orientações de uso. Escreva em português do Brasil, a menos que o público da API peça outra língua; mantenha em inglês nomes de campos, rotas e trechos de código.

Serve para APIs REST, GraphQL e WebSocket.

## Quando usar

- Documentar uma API nova.
- Atualizar a documentação de uma API existente.
- Preencher a falta de documentação.
- Preparar a entrada de novos desenvolvedores.
- Publicar a API para usuários externos ou parceiros.
- Criar uma especificação OpenAPI (Swagger).

## Regra principal: documente o que o código faz

Leia o código antes de escrever. Toda rota, campo, regra de validação, código de status e mensagem de erro precisa existir no código ou em testes. Não invente endpoints, campos ou limites. Se algo não estiver claro (limite de requisições, prazo de expiração de token, regra de negócio), pergunte ou marque com `[confirmar]`.

## Passo a passo

### 1. Entenda a estrutura da API
Procure no código:
- Rotas e endpoints (arquivos de rotas, controllers, decorators, resolvers).
- Métodos HTTP (GET, POST, PUT, PATCH, DELETE).
- Parâmetros e corpo da requisição (DTOs, schemas, validações).
- Formatos de resposta e códigos de status.
- Autenticação e permissões (middlewares, guards).
- Tratamento de erros.
- Especificações e coleções já existentes (OpenAPI, Postman, testes de integração).

### 2. Documente cada endpoint

**Dados do endpoint:** método e caminho, o que faz em uma frase, autenticação e permissão exigidas, limite de requisições (se houver).

**Requisição:** parâmetros de caminho, parâmetros de consulta (query), cabeçalhos e corpo, com tipo, se é obrigatório, regras de validação e exemplo de cada campo.

**Resposta:** código e corpo de sucesso, todas as respostas de erro possíveis e cabeçalhos importantes.

**Exemplos de código:** cURL, JavaScript/TypeScript (fetch ou axios), Python (requests) e outras linguagens, se o público usar.

### 3. Orientações de uso
- Primeiros passos.
- Configuração da autenticação.
- Casos de uso comuns.
- Boas práticas.
- Limites de requisição.
- Paginação, filtros e ordenação.

### 4. Erros
- Todos os códigos de erro e o formato da resposta de erro.
- Guia de solução de problemas e cenários comuns.

### 5. Recursos para testar
Quando fizer sentido: especificação OpenAPI, coleção do Postman ou do Insomnia, exemplos de resposta e ambiente de testes (sandbox).

---

## Exemplos

### Exemplo 1: endpoint REST

````markdown
## Criar usuário

Cria uma conta de usuário.

**Endpoint:** `POST /api/v1/users`
**Autenticação:** obrigatória (token Bearer de administrador)

**Corpo da requisição:**

| Campo | Tipo | Obrigatório | Regras |
|---|---|---|---|
| `email` | string | sim | e-mail válido, único |
| `password` | string | sim | mínimo de 8 caracteres, 1 maiúscula e 1 número |
| `name` | string | sim | de 2 a 50 caracteres |
| `role` | string | não | `user` ou `admin`; padrão `user` |

```json
{
  "email": "maria.silva@exemplo.com",
  "password": "SenhaForte123",
  "name": "Maria Silva"
}
```

**Resposta de sucesso (201 Created):**

```json
{
  "id": "usr_1234567890",
  "email": "maria.silva@exemplo.com",
  "name": "Maria Silva",
  "role": "user",
  "createdAt": "2026-01-20T10:30:00Z",
  "emailVerified": false
}
```

**Erros:**

- `400 Bad Request`: dados inválidos
  ```json
  {"error": "VALIDATION_ERROR", "message": "Formato de e-mail inválido", "field": "email"}
  ```
- `401 Unauthorized`: token ausente ou inválido.
- `403 Forbidden`: o token não tem permissão para criar usuários.
- `409 Conflict`: e-mail já cadastrado
  ```json
  {"error": "EMAIL_EXISTS", "message": "Já existe uma conta com este e-mail"}
  ```

**cURL:**

```bash
curl -X POST https://api.exemplo.com/api/v1/users \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"email": "maria.silva@exemplo.com", "password": "SenhaForte123", "name": "Maria Silva"}'
```

**JavaScript:**

```javascript
const response = await fetch('https://api.exemplo.com/api/v1/users', {
  method: 'POST',
  headers: {
    Authorization: `Bearer ${token}`,
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    email: 'maria.silva@exemplo.com',
    password: 'SenhaForte123',
    name: 'Maria Silva',
  }),
});

if (!response.ok) {
  const erro = await response.json();
  throw new Error(`${response.status}: ${erro.message}`);
}
const usuario = await response.json();
```

**Python:**

```python
import requests

response = requests.post(
    "https://api.exemplo.com/api/v1/users",
    headers={"Authorization": f"Bearer {token}"},
    json={
        "email": "maria.silva@exemplo.com",
        "password": "SenhaForte123",
        "name": "Maria Silva",
    },
    timeout=10,
)
response.raise_for_status()
usuario = response.json()
```
````

Observação: as regras de cada campo ficam numa tabela, porque JSON não aceita comentários (`//`) e o leitor copiaria um exemplo inválido. Se a API aceita `role: "admin"`, documente quem pode enviar esse campo; se qualquer usuário puder, isso é uma falha de segurança a reportar, não a documentar.

### Exemplo 2: GraphQL

````markdown
## Consultar usuário

Busca os dados de um usuário pelo ID.

```graphql
query GetUser($id: ID!) {
  user(id: $id) {
    id
    email
    name
    posts {
      id
      title
      publishedAt
    }
  }
}
```

**Variáveis:**
```json
{"id": "usr_1234567890"}
```

**Resposta:**
```json
{
  "data": {
    "user": {
      "id": "usr_1234567890",
      "email": "maria.silva@exemplo.com",
      "name": "Maria Silva",
      "posts": [{"id": "post_123", "title": "Meu primeiro post", "publishedAt": "2026-01-21T14:00:00Z"}]
    }
  }
}
```

**Erro (usuário não encontrado):**
```json
{
  "data": {"user": null},
  "errors": [{"message": "Usuário não encontrado", "path": ["user"], "extensions": {"code": "USER_NOT_FOUND"}}]
}
```

Em GraphQL, erros costumam vir com status HTTP 200 e o campo `errors`; documente essa convenção.
````

### Exemplo 3: autenticação

````markdown
## Autenticação

Todas as requisições exigem um token Bearer.

### Obter um token
`POST /api/v1/auth/login`

```json
{"email": "maria.silva@exemplo.com", "password": "sua-senha"}
```

Resposta:
```json
{"accessToken": "eyJhbGciOi...", "expiresIn": 3600, "refreshToken": "rt_..."}
```

### Usar o token
```
Authorization: Bearer SEU_TOKEN
```

### Renovar
O token expira em 1 hora. Para obter um novo sem fazer login de novo:
`POST /api/v1/auth/refresh`
```json
{"refreshToken": "rt_..."}
```

Guarde os tokens em local seguro; nunca os coloque em URLs, logs ou repositórios.
````

---

## Boas práticas

### Faça
- **Seja consistente:** mesmo formato para todos os endpoints.
- **Dê exemplos que funcionam**, em mais de uma linguagem, e teste cada um.
- **Documente todos os erros** e o que significam.
- **Use dados realistas**, mas fictícios: nada de "foo" e "bar", nem dados pessoais reais, tokens ou senhas verdadeiras (LGPD e segurança).
- **Explique cada parâmetro** e suas restrições.
- **Versione a API** (no caminho, como `/api/v1/`, ou em cabeçalho) e documente a política de versões.
- **Mostre a data da última atualização.**
- **Ligue endpoints relacionados.**
- **Documente os limites de requisição.**
- **Ofereça uma coleção para testes** (Postman, Insomnia, Bruno).

### Não faça
- Pular os casos de erro.
- Descrições vagas ("Obtém dados" não ajuda).
- Esquecer a autenticação de algum endpoint.
- Ignorar paginação, filtros e ordenação.
- Deixar exemplos quebrados.
- Deixar a documentação desatualizada em relação ao código.
- Complicar: a documentação deve ser fácil de escanear.
- Esquecer cabeçalhos importantes da resposta (paginação, limites, `Location`).

## Estrutura recomendada

1. **Introdução:** o que a API faz, URL base, versão, contato de suporte.
2. **Autenticação:** como autenticar, gestão de tokens, boas práticas de segurança.
3. **Primeiros passos:** um exemplo simples e um caso de uso comum de ponta a ponta.
4. **Endpoints:** organizados por recurso, com todos os detalhes.
5. **Modelos de dados:** schemas, descrição dos campos, regras de validação.
6. **Erros:** referência dos códigos, formato da resposta, solução de problemas. Um formato padrão útil é o Problem Details (RFC 9457, `application/problem+json`).
7. **Limites de requisição:** cotas, cabeçalhos de controle, como lidar com o erro 429.
8. **Histórico de mudanças:** versões, mudanças incompatíveis, avisos de descontinuação.
9. **SDKs e ferramentas:** bibliotecas cliente, coleções de teste, especificação OpenAPI.

## Problemas comuns

| Problema | Sintomas | Solução |
|---|---|---|
| Documentação desatualizada | Exemplos não funcionam, parâmetros errados | Gerar a documentação a partir do código ou da especificação OpenAPI; testes que validam a especificação; revisar a documentação a cada mudança na API |
| Erros sem documentação | Usuários não sabem tratar falhas; mais chamados de suporte | Documentar cada código de erro, com mensagem, exemplo e como resolver |
| Exemplos que não funcionam | Usuários não conseguem começar | Testar cada exemplo; exemplos completos, não fragmentos; ambiente de testes |
| Parâmetros pouco claros | Muitas requisições inválidas | Marcar obrigatório e opcional, tipos, formatos, regras e valores de exemplo |

## Ferramentas e formatos

### OpenAPI

```yaml
openapi: 3.1.0
info:
  title: API de Exemplo
  version: 1.0.0
servers:
  - url: https://api.exemplo.com/api/v1
paths:
  /users:
    post:
      summary: Cria um usuário
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
      responses:
        '201':
          description: Usuário criado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '409':
          description: E-mail já cadastrado
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
  schemas:
    CreateUserRequest:
      type: object
      required: [email, password, name]
      properties:
        email: {type: string, format: email}
        password: {type: string, minLength: 8}
        name: {type: string, minLength: 2, maxLength: 50}
    User:
      type: object
      properties:
        id: {type: string}
        email: {type: string, format: email}
        name: {type: string}
        createdAt: {type: string, format: date-time}
```

Muitos frameworks geram a especificação a partir do código (FastAPI, NestJS com `@nestjs/swagger`, Spring com springdoc). Prefira isso a escrever à mão, e valide a especificação com um linter como o Spectral ou o Redocly CLI. Para publicar, Swagger UI, Redoc ou Scalar.

### Coleção do Postman

```json
{
  "info": {
    "name": "API de Exemplo",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "item": [
    {
      "name": "Criar usuário",
      "request": {
        "method": "POST",
        "url": "{{baseUrl}}/api/v1/users",
        "header": [{"key": "Content-Type", "value": "application/json"}],
        "body": {"mode": "raw", "raw": "{\"email\": \"maria.silva@exemplo.com\", \"password\": \"SenhaForte123\", \"name\": \"Maria Silva\"}"}
      }
    }
  ]
}
```

## Formato da resposta

Entregue a documentação no formato pedido (Markdown, OpenAPI ou ambos), organizada por recurso, e no fim uma lista do que ficou marcado com `[confirmar]` e de inconsistências encontradas no código (ex.: endpoint sem autenticação, erro não tratado).

## Referências

- [Especificação OpenAPI](https://spec.openapis.org/oas/latest.html)
- [Aprenda GraphQL](https://graphql.org/learn/)
- [RFC 9457: Problem Details](https://www.rfc-editor.org/rfc/rfc9457)
- [Documentação do Postman](https://learning.postman.com/docs/)

Dica: mantenha a documentação o mais perto possível do código. Ferramentas que geram a documentação a partir do código ou da especificação ajudam a manter tudo em sincronia.

---

## Créditos e licença

Skill adaptada e traduzida para o português a partir da skill `api-documentation-generator` do projeto [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills), sob a licença **MIT** (Copyright (c) 2026 Antigravity User). Encontrada na coletânea GovHub-br/gov-hub-skills (pasta `03-backend-apis/api-documentation-generator`), que não registra a origem.

Alterações desta versão: tradução para o português do Brasil, com exemplos em português; regra de documentar apenas o que existe no código, marcando dúvidas com `[confirmar]`; exemplo de requisição com comentários `//` (JSON inválido) trocado por tabela de campos; aviso sobre o campo `role` aberto no cadastro; inclusão do erro 403, do tratamento de erro nos exemplos de JavaScript e Python e de `timeout` no Python; exemplo de erro GraphQL completo; OpenAPI atualizado para 3.1 com segurança e respostas; Problem Details (RFC 9457), ferramentas de geração, validação e publicação; cuidados com dados pessoais, tokens e senhas nos exemplos; remoção das skills relacionadas que não existem neste hub e de um link de referência sem relação com o tema; seção "Formato da resposta".

Texto da licença original:

```
MIT License

Copyright (c) 2026 Antigravity User

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
