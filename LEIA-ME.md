# LANG Soluções Tecnológicas — publicação automática no Instagram

Este repositório guarda os vídeos e imagens de 26/10 a 09/11/2026 e publica sozinho, todo dia:

- **12h** — 1 story (vídeo curto com cenas reais da loja)
- **19h** — 1 post principal (reel, carrossel ou imagem), com a legenda já escrita

Nada precisa ser ativado no dia. Depois da configuração abaixo (feita uma vez), o GitHub publica sozinho.

## O que tem aqui
- `calendario.csv` — o que sai em cada dia e hora (id, data, horário, tipo, arquivos, frase, legenda). Dá para editar legenda e horário no próprio GitHub.
- `reels/`, `stories/`, `feed/` — as mídias. O GitHub Pages deixa elas públicas, e o Instagram busca por lá.
- `publicar.py` — script que publica pela API oficial da Meta.
- `postar.yml` — liga o script todo dia às 12h e 19h (Brasília). **Antes de usar, ele precisa ficar na pasta `.github/workflows/` do repositório** (veja o passo 3.1 abaixo).
- `postado.csv` — registro do que já foi publicado (criado sozinho).

## Configuração (uma vez só)

### 1) Conta do Instagram
O perfil @lang_solucoes_tecnologicas precisa ser **conta profissional** (Comercial ou Criador). No app: Configurações > Tipo de conta e ferramentas.

### 2) Aplicativo na Meta (gera o token)
1. Entre em developers.facebook.com com o Facebook de quem administra a página e crie um app do tipo **Empresa**.
2. Adicione o produto **Instagram** e abra **API com login do Instagram > Configuração da API**.
3. Adicione a conta @lang_solucoes_tecnologicas e clique em **Gerar token**. Copie o **token** e o **ID da conta do Instagram** que aparecem ali.
4. As permissões necessárias são `instagram_business_basic` e `instagram_business_content_publish`. Para publicar na sua própria conta, o app pode ficar em modo de desenvolvimento.

### 3) GitHub
1. Crie um repositório **público** chamado `lang-instagram` e envie todo o conteúdo desta pasta (GitHub Desktop: *Add local repository*, depois *Commit* e *Publish repository*).
1.1. **Mover o `postar.yml`** para `.github/workflows/postar.yml`: no GitHub, *Add file > Create new file*, digite `.github/workflows/postar.yml` como nome, cole o conteúdo do arquivo `postar.yml` e salve. Depois apague o `postar.yml` solto na raiz.
2. **Settings > Pages**: Source = *Deploy from a branch*, branch `main`, pasta `/ (root)`. Anote o endereço que aparece (`https://SEU-USUARIO.github.io/lang-instagram`).
3. **Settings > Secrets and variables > Actions**:

| Onde | Nome | Valor |
|---|---|---|
| Secrets | `IG_TOKEN` | o token do passo 2 |
| Secrets | `IG_USER_ID` | o ID da conta do passo 2 |
| Variables | `BASE_URL` | `https://SEU-USUARIO.github.io/lang-instagram` (sem barra no fim) |

### 4) Testar (aba Actions > postar-no-instagram > Run workflow)
1. modo `verificar` — confirma que o token e o ID funcionam. Não publica nada.
2. modo `simular` — lista o que publicaria hoje.
3. modo `publicar` com `id` = `S01` — publica o story do dia 26/10 agora (teste real). Depois pode apagar do perfil se quiser.

Pronto. A partir daí o GitHub roda sozinho às 12h e 19h.

## Se algo der errado
- "Arquivo não acessível": o GitHub Pages ainda não publicou (espere 2 minutos) ou a pasta não foi enviada.
- Erro de token: gere outro no painel do app e troque o segredo `IG_TOKEN`.
- O workflow avisa em amarelo se o Instagram devolver um token diferente ao renovar: troque o segredo pelo novo.
- Se um dia não rodar, o script recupera atrasos de até 2 dias; depois disso, ele pula para não despejar uma fila antiga.

## Observações
- As trilhas dos vídeos vêm de músicas que já estavam nos posts da página. O Instagram pode silenciar áudio com direitos autorais; se acontecer, troque o vídeo ou publique sem som.
- Stories publicados pela API não aceitam figurinhas interativas (enquete, link). Por isso o botão "Fale com a LANG" é só visual; o contato continua pelo link da bio.
