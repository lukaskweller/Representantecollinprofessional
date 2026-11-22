# Collin Dashboard - Integração com Supabase

## Arquivos do projeto
- `index.html`
- `styles.css`
- `app.js`
- `service-worker.js`
- `manifest.webmanifest`
- `database.sql`
- `assets/` (ícones e imagens)

## Como ativar o banco (Supabase)
1. Crie um projeto em https://supabase.com.
2. Em **SQL Editor**, cole o conteúdo de `database.sql` e clique em **Run**.
   - Se o banco já existir, o script pode ser rodado de novo: ele recria as políticas.
3. Crie os usuários em **Authentication > Users** e insira o registro correspondente na tabela `perfis` (mesmo `id` do usuário).
4. Em **Project Settings > API**, copie a `Project URL` e a `anon public key`.
5. No `app.js`, preencha `SUPABASE_URL` e `SUPABASE_KEY` (linhas 2 e 3).
6. Envie os arquivos atualizados ao GitHub.

## Segurança
- Nunca coloque senhas ou chaves `service_role` no front-end.
- A chave `anon` pode ficar no `app.js`, pois a proteção dos dados vem do RLS.
- Dados de clientes não são guardados em cache offline.

## Teste rápido
- Login com um representante: deve ver somente a própria carteira.
- Login com um gerente: deve ver todas as carteiras.
