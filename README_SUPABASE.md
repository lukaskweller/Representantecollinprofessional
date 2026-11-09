# 🚀 Atualização Collin Dashboard - Integração com Banco de Dados

Esta versão prepara o seu sistema para a transição de Planilhas Google para um Banco de Dados Profissional (Supabase), permitindo multiusuários (Representantes, Gerentes e Diretores) e garantindo a segurança dos dados.

## Como testar agora mesmo:
1. Faça o upload dos arquivos `index.html`, `styles.css` e `app.js` para o seu repositório no GitHub.
2. A aplicação continuará funcionando normalmente com os dados da planilha como "Fallback" (Plano B), para que você não fique sem sistema.

## Como ativar o Novo Banco de Dados (Supabase):
1. Crie uma conta gratuita em [Supabase](https://supabase.com/).
2. Crie um novo projeto.
3. No painel do Supabase, vá em **SQL Editor** e cole todo o conteúdo do arquivo `database.sql` fornecido nesta pasta. Clique em "Run".
4. Vá em **Project Settings > API**. Copie a `Project URL` e a `anon public key`.
5. Abra o arquivo `app.js` e cole essas chaves nas variáveis `SUPABASE_URL` e `SUPABASE_KEY` nas linhas 2 e 3.
6. Envie o `app.js` atualizado para o GitHub. A partir desse momento, o sistema lerá do banco de dados seguro!
