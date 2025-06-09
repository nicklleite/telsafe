
# TelSafe - Documentação Inicial

## ✨ Visão Geral

**TelSafe** é uma plataforma colaborativa que permite verificar se um número de telefone é confiável ou se já foi associado a golpes. O foco é a **privacidade dos usuários legítimos** e a **exposição controlada de possíveis fraudadores**, sem armazenar ou exibir números reais de forma aberta.

---

## 🎯 Objetivo

- Permitir que usuários verifiquem se um número é real ou suspeito.
- Proteger a privacidade dos números legítimos.
- Expor apenas números que foram marcados como golpe.
- Evitar registros abusivos ou mal-intencionados.

---

## 📦 Funcionalidades

### 🔎 Consulta de número

- Entrada: telefone (formato nacional ou internacional).
- O número é criptografado (hash + salt) e comparado com a base de dados.
- Três possíveis retornos:
  - ✅ Número confirmado como legítimo (sem exibir publicamente).
  - ❌ Número marcado como golpe (exibido com máscara e relatos).
  - 🚫 Número não encontrado.

---

### ➕ Cadastro de número

Dois tipos de registro:

1. **Confirmar número como próprio:**
   - Usuário afirma que aquele número é seu.
   - Registro **não é exibido publicamente.**
   - Serve apenas como validação interna futura.

2. **Denunciar número como golpe:**
   - Permite relatar tentativas de golpe.
   - Pode conter comentário opcional.
   - Se houver múltiplos relatos, o número é exibido publicamente com máscara.

---

## 🔐 Privacidade e segurança

- Telefones são armazenados apenas como `hash(telefone + salt)`.
- O salt pode ser rotativo ou fixo por ambiente.
- IPs também podem ser armazenados anonimizados para controle de abuso.
- Números marcados como “meus” **nunca são exibidos publicamente.**
- Apenas números com denúncias aparecem na base pública.

---

## 🧠 Regras de Exibição Pública

- Um número **só aparece publicamente se tiver mais de uma denúncia.**
- A exibição mostra o número **parcialmente mascarado**:
  - Exemplo: `(**) *****-34**`
- Comentários ficam visíveis (sem identificação do denunciante).
- Relatos únicos não são exibidos, mas são armazenados internamente.

---

## 📁 Estrutura de Dados Inicial

Tabela: `telsafe_reports`

| Campo         | Tipo                      | Descrição                             |
|---------------|---------------------------|----------------------------------------|
| id            | UUID / Integer            | ID do registro                        |
| hashed_number | String (SHA-256 + salt)   | Hash do telefone informado            |
| type          | Enum: 'confirmed'/'scam'  | Tipo de marcação                      |
| comment       | Text (nullable)           | Comentário opcional                   |
| created_at    | Timestamp                 | Data da criação                       |
| ip_address    | String (hashed/anônimo)   | IP de origem da denúncia              |

---

## 🛡️ Medidas Antifraude (para MVP)

- Limite de denúncias por IP (ex: 3/dia).
- Uso de CAPTCHA no envio.
- Não exibir denúncias únicas.
- Sal rotativo para evitar engenharia reversa.

---

## 💡 Futuras Melhorias (Planejadas)

- Verificação por SMS para quem quiser confirmar seu número.
- Sistema de pontuação de confiança dos relatos.
- Painel de contestação de denúncias.
- E-mails opcionais para rastreabilidade leve.

---

## 📛 Nome do Projeto

**TelSafe** foi escolhido por:
- Transmitir ideia de segurança telefônica.
- Nome direto, com sonoridade confiável.
- Fácil de lembrar, neutro e com potencial internacional.

---

## ❗️Diferença entre TelSafe e identificadores de chamada (ex: Whoscall)

Enquanto aplicativos como **Whoscall**, **Truecaller** e similares atuam como **identificadores de chamada** em tempo real, geralmente através da instalação de apps com acesso à agenda e permissões amplas, o **TelSafe** propõe algo **muito mais simples, direto e privado**:

| Característica                         | Identificadores (Whoscall, etc)    | TelSafe                                |
|----------------------------------------|-------------------------------------|-----------------------------------------|
| Tipo de serviço                        | Identificador de chamada            | Consulta pontual por número             |
| Forma de uso                           | App instalado no celular            | Web/app com busca manual                |
| Privacidade                            | Acesso à agenda e chamadas          | Sem coleta de dados sensíveis           |
| Armazenamento de números               | Base extensa, possivelmente vendida | Apenas hash dos números                 |
| Objetivo principal                     | Identificar chamadas e spam         | Confirmar se número é golpe ou real     |
| Dados exibidos                         | Nome, operadora, tags de spam       | Apenas se for golpe e com máscara       |
| Enfoque principal                      | Bloqueio/identificação de ligações  | Consulta pontual de veracidade          |

**TelSafe** não tem o objetivo de substituir identificadores de chamada. Ele é uma camada extra de verificação rápida, **com foco em proteção contra golpes e sem comprometer a privacidade dos usuários legítimos.**

---

## 🧭 Missão

Reduzir golpes por aplicativos de mensagens com uma base colaborativa e segura, onde qualquer pessoa possa consultar um número e descobrir — de forma anônima — se ele já foi usado em tentativas de fraude.

---

## 📌 Status Atual

- [x] Escopo inicial definido
- [x] Nome validado
- [x] Funcionalidades básicas especificadas
- [x] Estratégia de privacidade definida
- [x] Regras contra abuso documentadas
- [ ] Estrutura técnica implementada
- [ ] MVP desenvolvido
- [ ] Validação de mercado
