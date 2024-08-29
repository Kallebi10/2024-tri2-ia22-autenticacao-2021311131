# 2024-tri2-ia22-autenticacao-2021311131

Ótimo! Vamos abordar os conceitos de forma mais técnica.

### **Autenticação vs. Autorização**

#### **Autenticação (Authentication)**

A autenticação é o processo de verificar a identidade de um usuário ou sistema. Basicamente, o objetivo é garantir que o usuário é realmente quem ele diz ser. Esse processo geralmente envolve a validação de credenciais, como:

- **Algo que você sabe:** Senha, PIN, ou respostas a perguntas de segurança.
- **Algo que você tem:** Um token físico, cartão, ou um dispositivo móvel.
- **Algo que você é:** Biometria, como impressões digitais, reconhecimento facial, ou íris.

Em termos técnicos, quando um usuário tenta acessar um sistema, ele envia suas credenciais (como nome de usuário e senha) para o servidor. O servidor então compara essas credenciais com as armazenadas em seu banco de dados. Se houver uma correspondência, o usuário é autenticado e permitido a prosseguir. 

- **Exemplo técnico:** Em um sistema web, a autenticação pode ser gerenciada por um serviço como OAuth, onde o usuário fornece um token de acesso que é validado para confirmar sua identidade.

#### **Autorização (Authorization)**

A autorização ocorre após a autenticação e envolve a determinação dos privilégios ou permissões que o usuário autenticado tem dentro do sistema. Em outras palavras, a autorização define o que o usuário pode ou não pode fazer.

Tecnicamente, a autorização pode ser implementada utilizando várias abordagens, como:

- **Controle de Acesso Baseado em Funções (RBAC):** Onde as permissões são associadas a papéis específicos, e os usuários são atribuídos a esses papéis. Por exemplo, um usuário com o papel de "Administrador" pode ter acesso a todas as funcionalidades do sistema, enquanto um usuário com o papel de "Usuário Comum" pode ter acesso limitado.
- **Controle de Acesso Baseado em Atributos (ABAC):** Onde a autorização é decidida com base em atributos associados ao usuário, ao recurso que ele quer acessar, ou ao ambiente em que a solicitação é feita.

- **Exemplo técnico:** Em um sistema RESTful, a autorização pode ser realizada verificando o escopo de um token JWT (JSON Web Token) para determinar se o usuário tem permissão para acessar um endpoint específico.

#### **Resumo Técnico:**
- **Autenticação:** Valida a identidade do usuário.
- **Autorização:** Define as permissões e o nível de acesso do usuário.

Ambos são processos cruciais na segurança de sistemas e são frequentemente combinados para garantir que apenas usuários autenticados possam realizar ações específicas com base nas suas permissões.

### **Autenticação com Token (JWT)**

A Autenticação com Token é uma técnica amplamente usada em aplicações web e APIs modernas. Vou explicar como ela funciona, por que é útil e como ela se compara a outras formas de autenticação.

### **O que é um Token?**
Um **token** é um pequeno pedaço de dados que representa alguma informação, como a identidade de um usuário ou as permissões de acesso. Na autenticação com token, esse token é gerado após a validação bem-sucedida das credenciais do usuário (nome de usuário e senha, por exemplo) e é utilizado para acessar recursos protegidos sem a necessidade de enviar as credenciais repetidamente.

### **Como Funciona a Autenticação com Token?**

#### 1. **Login e Geração do Token:**
   - O usuário envia suas credenciais (nome de usuário e senha) para o servidor.
   - O servidor valida essas credenciais contra o banco de dados.
   - Se as credenciais forem válidas, o servidor gera um token único para o usuário. Esse token geralmente é um **JWT (JSON Web Token)**, que é amplamente utilizado por ser compacto e seguro.
   - O servidor envia o token de volta ao cliente (ex.: navegador ou aplicativo móvel).

#### 2. **Armazenamento do Token:**
   - O token é armazenado no lado do cliente, geralmente em **localStorage** ou **sessionStorage** (em aplicações web) ou em algum armazenamento seguro no caso de aplicativos móveis.

#### 3. **Envio do Token em Requisições:**
   - Toda vez que o usuário faz uma nova requisição ao servidor para acessar um recurso protegido (como visualizar uma página de perfil ou acessar dados sensíveis), o token é enviado no cabeçalho da requisição HTTP, geralmente no campo `Authorization`, precedido da palavra `Bearer` (ex.: `Authorization: Bearer <seu-token>`).

#### 4. **Validação do Token pelo Servidor:**
   - O servidor, ao receber a requisição com o token, verifica se o token é válido:
     - Ele verifica a assinatura do token para garantir que não foi alterado.
     - Ele verifica se o token não está expirado.
     - Ele verifica se o token foi emitido por um servidor confiável.
   - Se o token for válido, o servidor permite o acesso ao recurso solicitado.
   - Se o token for inválido ou expirado, o acesso é negado e o usuário pode ser solicitado a autenticar novamente.

#### 5. **Expiração e Renovação do Token:**
   - Tokens têm um tempo de vida limitado por questões de segurança. Depois de expirar, o usuário precisará autenticar novamente ou pode usar um **refresh token** (um token especial que permite a geração de um novo token de acesso sem precisar reautenticar o usuário).

### **Vantagens da Autenticação com Token:**

- **Escalabilidade:** Como o servidor não precisa manter o estado de cada sessão de usuário, o sistema é mais fácil de escalar horizontalmente (adicionar mais servidores para suportar mais usuários).
- **Segurança:** Tokens podem ser projetados para expirar rapidamente e são auto-contidos, ou seja, contêm todas as informações necessárias para a autenticação, sem a necessidade de consultar um banco de dados constantemente.
- **Flexibilidade:** Tokens podem ser usados para autenticar e autorizar em múltiplos domínios e plataformas (web, mobile, APIs).

### **Comparação com Sessões Tradicionais:**

Na autenticação tradicional baseada em sessões:
- O servidor mantém o estado da sessão do usuário em memória ou banco de dados.
- O cliente recebe um cookie de sessão que é enviado com cada requisição subsequente.
- O servidor precisa verificar o cookie e associá-lo à sessão armazenada.

Na autenticação com token:
- Não há necessidade de armazenar o estado da sessão no servidor.
- O token é enviado com cada requisição e contém todas as informações necessárias para autenticação e autorização.

### **Exemplo de Autenticação com JWT (JSON Web Token):**

Um JWT é composto por três partes:
1. **Header:** Contém informações sobre o tipo de token e o algoritmo usado para assinar o token.
2. **Payload:** Contém as reivindicações (claims), que são as informações sobre o usuário e outros dados. Pode incluir coisas como `sub` (identificador do usuário), `iat` (data de emissão), `exp` (data de expiração), etc.
3. **Signature:** É a parte que garante que o token não foi alterado. É gerada combinando o header e o payload com uma chave secreta, usando o algoritmo especificado no header.

Um JWT codificado pode parecer algo assim:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

Esse token é então usado em todas as requisições subsequentes para garantir que o usuário está autenticado.

### **Considerações de Segurança:**
- **Armazenamento Seguro:** Tokens devem ser armazenados de maneira segura para evitar ataques como XSS (Cross-Site Scripting).
- **Expiração Curta:** Tokens devem ter um tempo de expiração curto para minimizar o impacto caso sejam comprometidos.
- **Revogação:** Embora tokens sejam projetados para serem stateless, mecanismos de revogação devem ser considerados, como a inclusão de uma lista negra (blacklist) de tokens inválidos.

A autenticação com token, especialmente usando JWT, é uma prática poderosa e eficiente para garantir a segurança em aplicações modernas, facilitando a autenticação distribuída e o acesso controlado em diferentes sistemas e plataformas.
