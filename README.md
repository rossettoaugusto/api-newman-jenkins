# 🛒 FakeStore API — Testes Automatizados com Newman + Jenkins

Esse projeto nasceu como um desafio técnico de QA e virou um exemplo bem bacana de como integrar testes de API num pipeline de CI/CD usando ferramentas do dia a dia de qualquer time de qualidade.

A ideia é simples: pegar a [FakeStore API](https://fakestoreapi.com) — uma API pública de e-commerce — e cobrir os principais fluxos com testes automatizados que rodam tanto na sua máquina quanto direto no Jenkins.

---

## 🧪 O que está sendo testado?

A coleção cobre três níveis de complexidade:

**Básicos**
- `API-001` — Lista todos os produtos e valida que o array não está vazio
- `API-002` — Busca um produto por ID e valida campos obrigatórios

**Intermediários**
- `API-003` — Cria um novo produto via POST e valida o ID e preço na resposta
- `API-004` — Atualiza um produto via PUT e confere os novos dados
- `API-005` — Deleta um produto e valida que a resposta traz o objeto removido

**Avançados**
- `API-007` — Faz login com credenciais válidas e valida o retorno do token JWT
- `API-008` — Filtra produtos por categoria e valida que a listagem não está vazia

No total são **7 requests** e **18+ assertions** cobrindo status codes, campos de resposta, tempo de resposta e tokens de autenticação.

---

## 🗂️ Estrutura do projeto

```
api-newman-jenkins/
├── collections/
│   └── desafio_augusto_rossetto.json   # collection do Postman exportada
├── results/                            # gerado automaticamente, ignorado pelo git
│   ├── newman-results.xml              # relatório JUnit pro Jenkins
│   └── newman-report.html             # relatório visual do htmlextra
├── .gitignore
├── Jenkinsfile                         # pipeline declarativo
├── package.json
└── README.md
```

---

## 🚀 Pré-requisitos

Antes de rodar, garante que você tem isso instalado:

- [Node.js](https://nodejs.org) v18+
- [Newman](https://www.npmjs.com/package/newman) — CLI do Postman
- [Jenkins LTS](https://www.jenkins.io) — pra rodar o pipeline
- Plugin **NodeJS** no Jenkins
- Plugin **HTML Publisher** no Jenkins (pra ver o relatório bonitinho)

---

## ⚙️ Instalação

Clona o projeto e instala as dependências:

```bash
git clone https://github.com/rossettoaugusto/api-newman-jenkins.git
cd api-newman-jenkins
npm install
```

---

## ▶️ Rodando localmente (sem Jenkins)

Se quiser rodar na mão direto no terminal:

```bash
npx newman run collections/desafio_augusto_rossetto.json \
  --reporters cli,junit,htmlextra \
  --reporter-junit-export results/newman-results.xml \
  --reporter-htmlextra-export results/newman-report.html
```

O resultado aparece no terminal e os relatórios ficam salvos na pasta `results/`.

---

## 🏗️ Rodando pelo Jenkins

### 1. Sobe o Jenkins localmente

```bash
brew services start jenkins-lts
```

Acessa em: [http://localhost:8080](http://localhost:8080)

### 2. Configura o Node.js no Jenkins

`Manage Jenkins → Tools → NodeJS installations → Add NodeJS`

Dá o nome `node-18` e salva.

### 3. Cria o job

- Novo item → **Pipeline**
- Em *Pipeline Definition* seleciona **Pipeline script from SCM**
- SCM: Git → URL: `https://github.com/rossettoaugusto/api-newman-jenkins.git`
- Branch: `*/main`
- Script Path: `Jenkinsfile`
- Salva e clica em **Construir agora**

### 4. Vendo os resultados

Depois do build você tem três formas de ver o resultado:

- **Console Output** — log completo da execução em tempo real
- **Test Results** — aba com o resumo JUnit dos testes
- **Newman Report** — relatório HTML visual com gráficos, tempos de resposta e detalhes de cada assertion (precisa do HTML Publisher Plugin)

---

## 📊 Exemplo de resultado esperado

```
7 requests     → 0 failed
18 assertions  → 0 failed
Tempo médio de resposta: ~291ms
Finished: SUCCESS ✅
```

---

## 🛠️ Tecnologias usadas

| Ferramenta | Papel |
|---|---|
| [Postman](https://postman.com) | Criação e organização dos testes |
| [Newman](https://github.com/postmanlabs/newman) | Execução via CLI |
| [newman-reporter-htmlextra](https://github.com/DannyDainton/newman-reporter-htmlextra) | Relatório HTML avançado |
| [Jenkins LTS](https://www.jenkins.io) | Pipeline de CI/CD |
| [FakeStore API](https://fakestoreapi.com) | API alvo dos testes |

---

## 👨‍💻 Autor

Feito por **Augusto Rossetto** como parte de um desafio técnico de QA.

Se tiver alguma dúvida ou sugestão, abre uma issue ou me chama! 🤙