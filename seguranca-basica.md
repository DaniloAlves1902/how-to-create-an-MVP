# 🌍 Language / Idioma
- [🇧🇷 Português](#-segurança-básica-para-mvps)
- [🇺🇸 English](#-basic-security-for-mvps)

---

<br>

<a id="-segurança-básica-para-mvps"></a>
# 🇧🇷 Segurança Básica para MVPs

Mesmo em um Produto Mínimo Viável, **dados de clientes não são brincadeira**. Você não precisa construir uma arquitetura de segurança de nível bancário no primeiro dia, mas você também não pode ignorar as defesas mais básicas. Um vazamento de dados ou a queda do servidor na semana de lançamento pode matar o seu MVP antes mesmo de ele tracionar.

Aqui estão os conceitos de segurança que você não pode pular:

## 1. SQL Injection (Injeção de SQL)
**O que é:** É o ataque mais antigo e comum da web. Acontece quando um usuário malicioso insere comandos de banco de dados diretamente nos campos de formulário (como no login ou barra de pesquisa) na tentativa de manipular ou destruir seus dados (ex: `DROP TABLE users;`).
**Como evitar:** Nunca (jamais!) pegue um texto que o usuário digitou e concatene diretamente na sua *query* do banco de dados. 
- Use *Prepared Statements*.
- A forma mais fácil hoje é utilizar ORMs (como Prisma, Drizzle ou TypeORM) que já higienizam as entradas por padrão.
- Se você usa ferramentas modernas como Supabase, as chamadas de API feitas pelos SDKs oficiais já protegem contra SQL Injection nativamente.

## 2. Rate Limiting (Limite de Taxa)
**O que é:** Imagine que um concorrente ou um bot malicioso resolva enviar 10.000 requisições por segundo contra a sua API. Se você não tiver limites, isso pode derrubar o seu servidor (ataque DDoS) ou, pior, fazer você receber uma fatura de nuvem astronômica no fim do mês por consumo de banda e computação.
**Como evitar:** Implemente um *Rate Limiter* (Limitador de Taxa).
- No Node.js (Express), você pode instalar a biblioteca `express-rate-limit` para limitar a, digamos, 100 requisições a cada 15 minutos por IP.
- Plataformas como a Vercel e o Cloudflare possuem opções gratuitas e nativas para configurar Rate Limits diretamente nas rotas antes mesmo da requisição chegar ao seu código.

## 3. Autenticação Segura (Não crie do zero)
**A Regra de Ouro:** Nunca crie o seu próprio sistema de criptografia de senhas.
Salvar senhas em texto puro ou usar *hashes* fracos (como MD5) é um erro fatal. Além disso, gerenciar o roubo de sessões, renovação de tokens e envio de e-mails de recuperação é muito complexo.
**A Solução:** Use serviços focados em autenticação como Supabase Auth, Firebase Auth, Clerk ou NextAuth. Eles lidam com toda a complexidade de gerar tokens JWT seguros, gerenciar cookies (`HttpOnly` e `Secure`) e proteger contra ataques CSRF e XSS na sessão do usuário.

## 4. Gerenciamento de Variáveis de Ambiente (.env)
Este é um erro clássico de iniciantes: fazer o *commit* de senhas, chaves de API (como a chave da OpenAI, Stripe ou a URL do banco de dados) diretamente no código e enviar para o GitHub.
Se o seu repositório for público, bots raspadores varrem o GitHub procurando por chaves do Stripe ou AWS para roubar.
**A Solução:** Mantenha qualquer informação sensível em um arquivo `.env` e garanta que este arquivo esteja listado no `.gitignore`. Quando for fazer o deploy (na Vercel ou Render, por exemplo), você insere essas chaves através do painel da própria plataforma.

---

<br>

<a id="-basic-security-for-mvps"></a>
# 🇺🇸 Basic Security for MVPs

Even in a Minimum Viable Product, **customer data is no joke**. You don't need to build bank-level security architecture on day one, but you cannot ignore basic defenses either. A data leak or a server crash on launch week can kill your MVP before it even gains traction.

Here are the security concepts you cannot skip:

## 1. SQL Injection
**What it is:** It is the oldest and most common web attack. It happens when a malicious user inserts database commands directly into form fields (like login or search bars) in an attempt to manipulate or destroy your data (e.g., `DROP TABLE users;`).
**How to avoid it:** Never (ever!) take text typed by a user and concatenate it directly into your database query.
- Use Prepared Statements.
- The easiest way today is to use ORMs (like Prisma, Drizzle, or TypeORM) which sanitize inputs by default.
- If you use modern tools like Supabase, API calls made by official SDKs already protect against SQL Injection natively.

## 2. Rate Limiting
**What it is:** Imagine a competitor or a malicious bot deciding to send 10,000 requests per second against your API. If you have no limits, this can crash your server (DDoS attack) or, worse, land you an astronomical cloud computing bill at the end of the month.
**How to avoid it:** Implement a Rate Limiter.
- In Node.js (Express), you can install the `express-rate-limit` library to restrict traffic to, say, 100 requests every 15 minutes per IP.
- Platforms like Vercel and Cloudflare have free, native options to configure Rate Limits directly on routes before the request even hits your code.

## 3. Secure Authentication (Don't build from scratch)
**The Golden Rule:** Never create your own password encryption system.
Saving passwords in plain text or using weak hashes (like MD5) is a fatal error. Moreover, managing session hijacking, token renewal, and recovery emails is highly complex.
**The Solution:** Use authentication-focused services like Supabase Auth, Firebase Auth, Clerk, or NextAuth. They handle the complexity of generating secure JWT tokens, managing cookies (`HttpOnly` and `Secure`), and protecting against CSRF and XSS attacks on the user's session.

## 4. Environment Variables Management (.env)
This is a classic beginner's mistake: committing passwords and API keys (like OpenAI, Stripe keys, or database URLs) directly into the code and pushing them to GitHub.
If your repository is public, scraper bots scan GitHub looking for Stripe or AWS keys to steal.
**The Solution:** Keep any sensitive information in a `.env` file and ensure this file is listed in your `.gitignore`. When deploying (on Vercel or Render, for instance), you input these keys directly through the platform's dashboard.
