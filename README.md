# 📌 Documentação Técnica - MVP: Cadastro de Propriedade

## 👥 Equipe
- **Front-end (2 devs)**: Implementação das telas e integração com API
- **Back-end (1 dev)**: Criação da API, banco de dados e regras de negócio
- **Tester (1)**: Garantia de aderência à documentação, padronização e qualidade

---

## 🧭 Escopo do MVP (Fase 1)
- Autenticação (Login/Cadastro)
- Tela de Dashboard Inicial
- Cadastro de Propriedade
- Tela de Gerenciamento da Propriedade (sem funcionalidades ativas)
- Controle de permissões (usuário comum e proprietário master)

---

## 🔐 Autenticação

### 🔸 Tela de Login - Colocar TOKEN de autenticação
**Campos:**
- `email` (string)
- `senha` (string)

**Botões:**
- Login
- Ir para cadastro

**Endpoint:**
```
POST /api/usuarios/login
```
**Body (JSON):**
```json
{
  "email": "user@email.com",
  "senha": "123456"
}
```

### 🔸 Tela de Cadastro
**Campos:**
- `nomeCompleto` (string)
- `email` (string)
- `senha` (string)
- `telefone` (string)
- `cpf` (string)
- `permissao` (string - padrão: "proprietario_master")

**Endpoint:**
```
POST /api/usuarios/cadastrar
```
**Body (JSON):**
```json
{
  "nomeCompleto": "João Silva",
  "email": "joao@email.com",
  "senha": "123456",
  "telefone": "11999999999",
  "cpf": "12345678900",
  "permissao": "proprietario_master"
}
```

---

## 🏠 Dashboard Inicial (Tela de Propriedades)
### 🧭 Layout
- Menu lateral (ícone hambúrguer à esquerda):
  - Foto do usuário
  - Opção “Home”
- Área principal:
  - Botão: Cadastrar Propriedade
  - Lista de propriedades (imagem + botão Gerenciar)
  - Campo de filtro por nome/cep/tipo

**Endpoint para listagem de propriedades:**
```
GET /api/propriedades?filtro=chacara
```
**Response (JSON):**
```json
[
  {
    "id": 1,
    "nomePropriedade": "Chácara Primavera",
    "imagemPrincipal": "url-da-imagem.jpg",
    "tipo": "Chácara",
    "cep": "12345-678"
  }
]
```

---

## 🏗️ Tela: Cadastro de Propriedade
### 📋 Campos do formulário:
- `nomePropriedade` (string, obrigatório)
- `fotos` (array de arquivos .jpg/.png, mínimo 1, máximo 15)
- `documento` (arquivo .pdf/.png com nome do proprietário e endereço)
- `cep` (string)
- `cidade` (string)
- `bairro` (string)
- `logradouro` (string)
- `numero` (string)
- `complemento` (string, opcional)
- `pontoReferencia` (string, opcional)
- `tipo` (enum: Casa, Apartamento, Lote, Chácara, Sítio, Cobertura, Kitnet, Terreno, Galpão)
- `valorEstimado` (float)

**Endpoint:**
```
POST /api/propriedades/cadastrar
```
**Content-Type:** multipart/form-data
**Payload (form):**
```form
nomePropriedade=Casa Verde
fotos=[img1.jpg, img2.jpg]
documento=conta_luz.pdf
cep=12345678
cidade=São Paulo
bairro=Centro
logradouro=Rua das Flores
numero=123
complemento=Apto 10
pontoReferencia=Próximo ao mercado
tipo=Casa
valorEstimado=450000.00
```

---

## 🧩 Tela: Gerenciamento da Propriedade
### ⚙️ Layout Inicial
- Exibição dos dados da propriedade
- Botões (sem funcionalidade nesta fase):
  - Gerenciar Cotistas (visível apenas para Proprietário Master)
  - Financeiro
  - Inventário
  - Agendamento de Uso

---

## 🔐 Permissões
### Níveis de acesso:
- **Proprietário Master**:
  - Pode cadastrar e gerenciar propriedades
  - Pode editar permissões
  - Pode acessar botão “Gerenciar Cotistas”

- **Usuário Comum**:
  - Apenas visualiza propriedades (nas fases futuras)

---

## 🔗 Integração Front <-> Back
- Padrão de variáveis JSON: `camelCase`
- Exemplo: `valorEstimado`, `pontoReferencia`, `documento`

---

## 📋 Tabela de Atributos - Cadastro de Propriedade
| Campo                | Nome no JSON       | Tipo de dado        | Obrigatório |
|---------------------|--------------------|----------------------|-------------|
| Nome da Propriedade | nomePropriedade    | string               | Sim         |
| Valor Estimado      | valorEstimado      | number (float)       | Sim         |
| Tipo de Propriedade | tipo               | string (enum)        | Sim         |
| CEP                 | cep                | string               | Sim         |
| Cidade              | cidade             | string               | Sim         |
| Bairro              | bairro             | string               | Sim         |
| Logradouro          | logradouro         | string               | Sim         |
| Número              | numero             | string               | Sim         |
| Complemento         | complemento         | string               | Não         |
| Ponto de Referência| pontoReferencia     | string               | Não         |
| Documento           | documento           | string (base64/URL) | Sim         |
| Fotos               | fotos              | array de strings     | Sim (min. 1)|

---

## 👤 Tabela de Atributos - Cadastro de Usuário
| Campo         | Nome no JSON   | Tipo de dado          | Obrigatório |
|---------------|----------------|------------------------|-------------|
| Nome completo | nomeCompleto   | string                 | Sim         |
| E-mail        | email          | string                 | Sim         |
| Senha         | senha          | string                 | Sim         |
| Telefone      | telefone       | string                 | Sim         |
| CPF           | cpf            | string                 | Sim         |
| Permissão     | permissao      | string (enum)          | Sim         |
| Foto de Perfil| fotoPerfil     | string (base64/URL)    | Não         |

---

## ✅ Regras de Validação
- Ao menos 1 foto obrigatória
- Documento obrigatório
- CEP deve ser válido (formato brasileiro)
- `valorEstimado` > 0
- CPF e e-mail devem ser únicos
- Apenas proprietário master pode criar propriedades

---

## 🔁 Endpoints - Usuários

### 📌 Cadastro
```
POST /api/usuarios/cadastrar
```
### 📌 Login
```
POST /api/usuarios/login
```
### 📌 Exibir usuário
```
GET /api/usuarios/{id}
```
### 📌 Editar usuário
```
PUT /api/usuarios/{id}
```
### 📌 Excluir usuário
```
DELETE /api/usuarios/{id}
```
### 📌 Alterar permissão
```
PUT /api/usuarios/{id}/permissao
```

---

## 🏡 Endpoints - Propriedades
### 📌 Cadastro
```
POST /api/propriedades/cadastrar
```
### 📌 Exibir
```
GET /api/propriedades/{id}
```
### 📌 Editar
```
PUT /api/propriedades/{id}
```
### 📌 Excluir
```
DELETE /api/propriedades/{id}
```
### 📌 Listar
```
GET /api/propriedades
```
### 📌 Adicionar Fotos
```
POST /api/propriedades/{id}/fotos
```
### 📌 Validar Documento
```
POST /api/propriedades/{id}/validarDocumento
```

---

## 🔐 Segurança
- Endpoints exigem autenticação via JWT (exceto cadastro/login)
- Controle de permissões por token
- Validação de dados obrigatória

---

## 🗃️ Banco de Dados

### Tabela: usuarios
```sql
CREATE TABLE usuarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nomeCompleto VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    senha VARCHAR(255) NOT NULL,
    telefone VARCHAR(15),
    cpf VARCHAR(14) UNIQUE NOT NULL,
    permissao ENUM('proprietario_master', 'cotista') NOT NULL,
    dataCadastro DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### Tabela: propriedades
```sql
CREATE TABLE propriedades (
    id INT AUTO_INCREMENT PRIMARY KEY,
    idUsuario INT,
    nomePropriedade VARCHAR(255) NOT NULL,
    enderecoCep VARCHAR(10),
    enderecoCidade VARCHAR(255),
    enderecoBairro VARCHAR(255),
    enderecoLogradouro VARCHAR(255),
    enderecoNumero VARCHAR(10),
    enderecoComplemento VARCHAR(255),
    enderecoPontoReferencia VARCHAR(255),
    tipo ENUM('Casa', 'Apartamento', 'Chacara', 'Lote', 'Outros') NOT NULL,
    valorEstimado DECIMAL(15, 2),
    documento VARCHAR(255),
    dataCadastro DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (idUsuario) REFERENCES usuarios(id)
);
```

### Tabela: fotos_propriedade
```sql
CREATE TABLE fotos_propriedade (
    id INT AUTO_INCREMENT PRIMARY KEY,
    idPropriedade INT,
    documento VARCHAR(255) NOT NULL,
    dataUpload DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (idPropriedade) REFERENCES propriedades(id)
);
```

### Tabela: documentos_propriedade
```sql
CREATE TABLE documentos_propriedade (
    id INT AUTO_INCREMENT PRIMARY KEY,
    idPropriedade INT,
    tipoDocumento ENUM('IPTU', 'Matrícula', 'Conta de Luz', 'Outros'),
    documento VARCHAR(255) NOT NULL,
    dataUpload DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (idPropriedade) REFERENCES propriedades(id)
);
```

---

## 🔗 Relacionamentos (ERD)
- `usuarios` 1:N `propriedades`
- `propriedades` N:1 `usuarios`
- `propriedades` 1:N `fotos_propriedade`
- `propriedades` 1:N `documentos_propriedade`

