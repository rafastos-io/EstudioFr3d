# Deploy do FilaOps na VPS pessoal

## Instância

- URL: https://fr3d.rafastos.com.br
- Plataforma: Coolify, projeto `FilaOps FR3D`, ambiente `production`
- Repositório: https://github.com/rafastos-io/EstudioFr3d, branch `main`
- Versão inicial: FilaOps `v4.4.0`
- Compose: `docker-compose.coolify.yml`
- Serviços: frontend (Nginx), backend (FastAPI), migrações e PostgreSQL 16

O Compose de produção não publica as portas do banco nem da API no host. O Coolify encaminha o domínio ao serviço `frontend` na porta interna `8080`; o Nginx encaminha `/api` e arquivos para o backend pela rede privada do Compose. Os dados do Postgres, arquivos enviados e identidade/segredos da instalação ficam em volumes Docker persistentes.

## Segredos e variáveis

`DB_PASSWORD` e `SECRET_KEY` são obrigatórios e devem existir como variáveis secretas da aplicação no Coolify. O `SECRET_KEY` assina as sessões de login; mantenha-o estável entre deploys. Não coloque valores secretos neste repositório, em commits ou neste documento.

O deploy também define `ENVIRONMENT=production`, `COOKIE_SECURE=true`, `FRONTEND_URL=https://fr3d.rafastos.com.br`, `ALLOWED_ORIGINS=https://fr3d.rafastos.com.br` e `ENABLE_PUBLIC_QUOTER=false`. Não há SMTP configurado; redefinições de senha por e-mail não estarão disponíveis até preencher as variáveis SMTP e `ADMIN_APPROVAL_EMAIL` com dados reais.

## Primeiro acesso

Na primeira visita, conclua imediatamente a criação da conta administradora no assistente do FilaOps. Faça isso antes de compartilhar o endereço. A criação inicial é disponibilizada pelo próprio FilaOps quando o banco ainda não tem usuário administrador.

## Deploys e atualizações

Commits enviados à branch `main` do repositório pessoal podem ser implantados pelo Coolify. Para atualizar o código do FilaOps, consulte primeiro a release upstream e suas notas; atualize a cópia local a partir de uma tag estável, preserve este Compose e este runbook, envie a mudança e então faça o redeploy no Coolify. A origem do projeto é `BLB3DPrinting/filaops`.

## Dados e recuperação

Os volumes `filaopsfr3d_pgdata`, `filaopsfr3d_uploads` e `filaopsfr3d_config` contêm dados persistentes. Preserve os três ao recriar ou atualizar a aplicação. Configure e verifique backups periódicos do PostgreSQL e mantenha uma cópia fora da VPS; o backup da VPS deve incluir os outros dois volumes para permitir recuperação completa.

Antes de restaurar ou remover volumes, confirme qual cópia será restaurada. Não use opções de remoção de volumes ao excluir/recriar o recurso.

## Licença

O código upstream é distribuído sob Business Source License 1.1. Esta instalação é self-hosted para uso próprio; o texto da licença restringe oferecer um SaaS comercial concorrente. Consulte [`LICENSE`](../LICENSE) antes de redistribuir ou alterar o modelo de uso.
