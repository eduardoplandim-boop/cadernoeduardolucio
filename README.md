# Caderno de Erros | Eduardo Landim

Aplicação web interativa para organização de erros de concursos públicos, revisão ativa (Segundo Ciclo), diagnóstico por origem do erro e Flashcards 3D com repetição espaçada.

## Funcionalidades
- **Dashboard Analítico:** métricas de superação, gráficos por matéria/prioridade e 32 mensagens motivacionais.
- **Cadastro e Filtros Combinados:** organização hierárquica por Matéria → Tópico, prioridade (Alta, Média, Superado) e origem obrigatória do erro.
- **Segundo Ciclo:** revisão ativa com ocultação da resposta e preservação do histórico.
- **Flashcards Interativos (3D):** repetição espaçada, atalhos de teclado e importação automática dos erros cadastrados.
- **Sincronização em Nuvem Multi-Dispositivo (Supabase):** persistência híbrida (`localStorage` + banco PostgreSQL do Supabase) para acessar e editar erros e flashcards de qualquer celular, tablet ou PC.
- **PWA e Backup JSON:** instalável no celular/PC via GitHub Pages e exportação/importação completa em JSON.

## Como ativar o Banco de Dados na Nuvem (Supabase)
1. Crie um projeto gratuito em [supabase.com](https://supabase.com).
2. No menu lateral do Supabase, abra **SQL Editor → New query**, cole o SQL abaixo e clique em **Run**:
```sql
create table if not exists public.caderno_sync (
  id text primary key,
  data jsonb not null default '{}'::jsonb,
  updated_at timestamptz not null default now()
);

alter table public.caderno_sync enable row level security;

drop policy if exists "Acesso publico ao caderno" on public.caderno_sync;
create policy "Acesso publico ao caderno"
  on public.caderno_sync
  for all
  to anon, authenticated
  using (true)
  with check (true);
```
3. Vá em **Project Settings → API** no Supabase e copie a **Project URL** e a chave pública **anon / publishable**.
4. Você pode conectar de duas maneiras:
   - **Pela interface do app:** abra a aba **Backup e configurações** e cole a `Project URL` e a `Anon Key`.
   - **Fixo no código (para conectar automaticamente em qualquer aparelho):** preencha as constantes `DEFAULT_SUPABASE_URL` e `DEFAULT_SUPABASE_ANON_KEY` no topo do `<script>` em `index.html`.

## Publicação no GitHub Pages
1. Envie todos os arquivos deste pacote para o repositório no GitHub.
2. Acesse **Settings → Pages**.
3. Em **Build and deployment**, selecione **Deploy from a branch**, escolha `main` e pasta `/ (root)` e clique em **Save**.
