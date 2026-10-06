# Pendências do Capítulo 10 (Resultados e Discussão)

Levantamento feito em 02/10/2026 a partir do código de `Camila/TCC-backend` e `GitHub/TCC-frontend`.
O capítulo 10 descreve só o que existe. Este texto reúne o que ficou de fora e o que precisa de ação antes da entrega.

> **Correções no código:** a descrição de cada problema de segurança, deploy e requisito, com onde
> está e como corrigir, está em [`correcoes_sistema.md`](correcoes_sistema.md).

---

## 0. Revisão de 02/10/2026: validação dos RNF, operação e deploy

### Marcadores criados no `capitulo10ResultadosDiscussao.tex`
Linhas conferidas em 02/10/2026 (mudam se o texto for editado; busque por `PREENCHER` e `APÓS DEPLOY`).

| Linha | Seção | Marcador |
|---|---|---|
| 605–606 | 10.7.1 Ambiente e Método | `% [APÓS DEPLOY]` + `[PREENCHER APÓS DEPLOY: tabela de latência em produção]` |
| 611 | 10.7.2 RNF-02 | `[PREENCHER: LCP p75 de 10 cargas da tela de Itens e da ficha, com tela.js]` |
| 613 | 10.7.2 RNF-06 | `[PREENCHER APÓS DEPLOY: mínimo, p50 e p95 de PATCH /item/:id e GET /item em produção]` |
| 615 | 10.7.2 RNF-07 | `[PREENCHER: p75, p98 e máximo das interações no editor, com editor.js]` |
| 617 | 10.7.2 RNF-08 | `[PREENCHER APÓS DEPLOY: monitoramento e disponibilidade medida]` |
| 647 | 10.7.3 Síntese da Validação (RNF-10) | `[PREENCHER APÓS DEPLOY: plano do Supabase e política de backup]` |
| 670 | 10.8.2 Fluxo de Implantação | `[PREENCHER APÓS DEPLOY: gerenciador de processo na VM]` |
| 671 | 10.8.2 Fluxo de Implantação | `[PREENCHER APÓS DEPLOY: domínio da API e Caddyfile]` |
| 672 | 10.8.2 Fluxo de Implantação | `[PREENCHER APÓS DEPLOY: URL da aplicação na Vercel]` |
| 684–685 | 10.8.2 Fluxo de Implantação | `% [APÓS DEPLOY]` + `[PREENCHER APÓS DEPLOY: situação dos quatro ajustes]` |
| 692–693 | 10.8.3 Operação da Plataforma | `% [APÓS DEPLOY]` + quatro marcadores: data de publicação, URL pública, domínio da API e VM, região e plano do Supabase |
| 697–698 | 10.8.3 Operação da Plataforma | `% [APÓS DEPLOY]` + `[PREENCHER APÓS DEPLOY: uso por terceiros, redação (a) ou (b)]` |
| 700–701 | 10.8.3 Operação da Plataforma | `% [APÓS DEPLOY]` + `[PREENCHER APÓS DEPLOY: o que foi observado em produção]` |
| 812–813 | 10.10.3 Discussão e Implicações | `% [APÓS DEPLOY]` + `[PREENCHER APÓS DEPLOY: frase sobre a publicação e o uso por terceiros]` |

Total: 17 marcadores `[PREENCHER...]` (2 sem deploy, 15 após o deploy) e 6 comentários `% [APÓS DEPLOY]`.

### Medições
- **Feitas (local, 02/10/2026):** latência de 8 rotas, n = 50 cada, todas as 441 requisições com
  sucesso. Arquivos brutos em `TCC-backend/scripts/medicoes/resultados/local-20261002T200334/`
  (`ambiente.json`, `preparo.jsonl`, `amostras.jsonl`, `resumo.json`, `resumo.md`, `limpeza.json`,
  `api.log`, `notas.md`). A conta de teste foi excluída e conferida (login → 401).
- **Pendentes, sem deploy (você faz no navegador):** carregamento de tela (`tela.js`) e editor
  (`editor.js`). O passo a passo está no `scripts/medicoes/README.md`. É preciso recriar a conta de
  teste com `preparar-dados.mjs` e excluí-la no fim com `limpar-dados.mjs`.
- **Pendentes, após o deploy:** as mesmas medições com `AMBIENTE=producao`; o monitor de
  disponibilidade; a confirmação do backup do Supabase.

### Achados desta revisão
- **A máquina das medições não é a do cap. 3:** foi o notebook (i5-13420H, 16 GB), não o desktop
  Ryzen 5 do `quad:equipamento`. O cap. 10 descreve a máquina real. Se o notebook também foi usado no
  desenvolvimento, vale acrescentá-lo ao quadro do cap. 3.
- **O cap. 9 (Implantação) foi escrito em 05/10/2026**, com o conteúdo de implantação que estava
  no cap. 10 (Arquitetura e Fluxo de Implantação). Ele descreve a produção real (Vercel, VM Oracle
  E2.1.Micro com Docker e Caddy, Supabase gratuito, R2, SMTP do Gmail, Duck DNS). Pendências do cap. 9:
  - `[PREENCHER]` do subdomínio Duck DNS (no `/etc/caddy/Caddyfile` da VM) e do endereço público
    `.vercel.app` (painel da Vercel > Domains; o endereço `...-git-main-...` exige login da Vercel);
  - comentários `% [DEPENDE DE CORREÇÃO]`: CORS só com `FRONTEND_URL`, `trust proxy` 1, `vercel.json`
    e porta `127.0.0.1:3000:3000` no `docker-compose.yml` (itens 2, 3 e 4 do `correcoes_sistema.md`);
  - comentários `% [VERIFICAR]`: IPv6 na VCN, rotina de `db dump`, `systemctl is-enabled docker caddy`
    e o `deploy.sh` (existe só na VM; não há GitHub Actions).
- **RNF-09 classificado como "Não atendido"**, com base no corte registrado da navegação do editor
  por teclado (WCAG 2.1.1, nível A).
- **Os itens 1 a 6 do `correcoes_sistema.md` bloqueiam ou prejudicam o deploy.**

---

## 1. O que não existe no sistema hoje

| Item | Situação no código | Onde aparece no texto do TCC |
|---|---|---|
| **Tela do feed (RF-22)** | A API tem `GET /social/feed` (paginação por cursor, só coleções acompanhadas e visíveis). O front-end não tem rota nem tela; o CHECKLIST marca o RF-22 como o único dos 24 não entregue. | Objetivo específico 6 (cap. 1), RF-22 (cap. 5), cap. 7 ("o *feed* de atualizações e as notificações são obtidos quando a interface os solicita") |
| **Troca de e-mail com verificação (RF-05)** | Não há rota nem modelo; `UpdateProfileDto` não aceita `email`. | Cap. 5 já lista como adiado |
| **Tempo real (RF-22/RF-24)** | Só REST; o sino consulta a cada 60 s. | Cap. 5, linha 192 ("atualização do feed em tempo real") |
| **Notificação do tipo `SYSTEM`** | O enum existe, mas nenhum serviço cria esse tipo. | Não citado no cap. 10 |
| **Envio de e-mail (SMTP)** | O CHECKLIST registra que o e-mail de redefinição foi testado em 26/08/2026 e não chegou. Sem `SMTP_*`, o envio falha em silêncio. | Cap. 10 cita o link de redefinição como funcionando: **confirmar que o e-mail chega antes da entrega** |
| **Encaixe em grade e *zoom* do editor** | Cortados de propósito (DECISOES §36 do front). | Não citados no cap. 10 |
| **Testes do front-end** | Nenhum teste automatizado. | Cap. 10 (10.7 e 10.8) registra isso |
| **Seed e teste e2e** | O `prisma/seed.ts` cria um usuário sem `userCode` e com senha sem hash; o `test/app.e2e-spec.ts` é o modelo gerado pelo Nest, importa um caminho que não existe e falharia. | Não citados no cap. 10 |
| **Medição dos RNF** | Nenhuma meta mensurável do cap. 5 foi medida (RNF-01 < 5 min, RNF-02 < 3 s, RNF-06 < 500 ms, RNF-07 < 200 ms, RNF-08 99%, RNF-09 WCAG 2.1 AA, RNF-10 backups). | Há um `% TODO (dado necessário)` na Subseção 10.8.2 |

### ⚠ Decisão necessária sobre o objetivo 6
O 6º objetivo específico (cap. 1) diz: "Implementar as funcionalidades sociais de amizade, acompanhamento de coleções, ***feed* de atualizações** e notificações". O cap. 10 não cita o feed. Antes da entrega, uma das duas:
1. implementar a tela do feed (a API já está pronta), ou
2. tirar "*feed* de atualizações" do objetivo no cap. 1 (e revisar o RF-22 no cap. 5 e a frase do cap. 7).

Há um `% TODO` no início da Seção 10.7 lembrando disso.

---

## 2. Inconsistências entre capítulos encontradas durante a escrita

| Capítulo | O que diz | O que o código mostra hoje |
|---|---|---|
| ~~Cap. 7 (Verificação) e cap. 8 (Resultados)~~ | ~~45 conjuntos, 277 testes~~ | **Resolvido em 02/10/2026:** caps. 7, 8 e 10 citam a mesma execução (51 conjuntos, 359 testes, todos aprovados, ≈ 8 s); o `quad:resumoTestes` foi refeito por módulo. |
| Cap. 7 (Servidor de Dados) | 36 migrações | 50 pastas de migração |
| Cap. 7 (Servidor de Aplicação) | 16 controladores, 81 rotas | Não reconferido; houve rotas novas depois (mescla, desassociação, foto com recorte etc.). Recontar. |
| Cap. 7 (Aplicação Cliente) | 13 telas | 19 telas em produção (mais `/movimento`, só em desenvolvimento): inclui Explorar, Amigos, Seguindo, perfil, categoria e coleção visitados |
| Cap. 6, linha 466 | A unidade é "a milésima parte da largura do quadro" | A folha tem largura configurável (200 a 4000); a unidade é 1/`sheetWidth`. Com a largura padrão (1000) coincide. O cap. 10 trata como generalização. |
| Cap. 6 | "13 tabelas" no texto | O `tab:tabelasIdentificadas` lista 14 (inclui `tags`) |
| Cap. 6, linha 246 | Usa `---` (travessão) | Reescrever com vírgula, dois-pontos ou parênteses |
| Cap. 5 (Requisitos Adiados) | "Descoberta pública" como adiada | A tela Explorar existe e é descrita no cap. 10 (10.2.1) |
| Cap. 5, linha 192 | Ator Sistema faz "atualização do feed em tempo real" | Não há tempo real nem tela de feed |
| Cap. 5, RF-06/RF-07 | Descrição até 300 caracteres | Agora 500 (anexo do `contexto_sistema.md`, 01/10/2026) |
| Item: nome | Banco `VarChar(255)` | DTO aceita no máximo 200. Não citado no texto; escolher um valor se algum capítulo mencionar. |
| README do back-end, linha 470 | "39 suítes" | 51 |
| Cap. 7, linha 159 | Os testes verificam "serviços, controladores e DTOs de todos os módulos" | O módulo `tag` não tem testes, e alguns controladores também não (`collection`, `tag`, `social-collection`, `social-item`, `item-layout-override`). Trocar "de todos os módulos" por "dos módulos". |

---

## 3. Capturas de tela a tirar (13 figuras)

Todas as figuras do cap. 10 estão com uma caixa `\fbox` "(pendente)" e a linha `\includegraphics` comentada logo abaixo. Ao ter a imagem: salvar em `imagens/Resultados/`, descomentar o `\includegraphics` e apagar a linha do `\fbox`.

| Rótulo | Arquivo sugerido | O que capturar |
|---|---|---|
| `fig:arquiteturaResultado` | `arquitetura.png` | **Diagrama a desenhar** (não é captura): navegador (React, Axios, TanStack Query) → API NestJS (throttler → JWT com sessão → DTOs → serviços → Prisma) → PostgreSQL; ao lado, armazenamento (R2 ou disco) e SMTP. Detalhes no comentário do `.tex`. |
| `fig:resultadoCategorias` | `categorias.png` | `/categorias` com 3 ou mais categorias com capa, contagens e resumo de privacidade; barra lateral aberta |
| `fig:resultadoColecoes` | `colecoes.png` | Coleções de uma categoria com as três privacidades e etiquetas visíveis |
| `fig:resultadoItens` | `itens.png` | Itens de uma coleção com a trilha "Categorias > Categoria > Coleção" |
| `fig:resultadoEditor` | `editor.png` | Editor em tela cheia com os seis tipos de Elemento e um selecionado (alças, giro, barra de propriedades, painel de posição/camadas) |
| `fig:resultadoFicha` | `ficha-item.png` | Ficha de item preenchida (sugestão: o "Duna" do cap. 5) |
| `fig:resultadoAparencia` | `aparencia.png` | `/aparencia` com as seis paletas padrão e a paleta em uso |
| `fig:resultadoEditorPaleta` | `editor-paleta.png` | Editor de paleta com prévia e, se possível, o aviso de contraste aparecendo |
| `fig:resultadoTemas` | `tema-claro.png` + `tema-escuro.png` | A mesma ficha nos dois temas, lado a lado (`\subfloat` já está pronto no comentário) |
| `fig:resultadoAmigos` | `amigos.png` | `/amigos` com um amigo e uma solicitação recebida |
| `fig:resultadoPerfil` | `perfil-visitado.png` | Perfil de outra pessoa visto por uma conta amiga |
| `fig:resultadoNotificacoes` | `notificacoes.png` | Painel do sino com notificação de amizade e de atualização, contador visível |
| `fig:resultadoMobile` | `mobile-fechado.png` + `mobile-gaveta.png` | Largura de celular (≈ 390 px): gaveta fechada e aberta |

Observação: as imagens de `Front/Front` e `TCC-frontend/Mockups` são protótipos (mostram "em breve" em Amigos e Feed, e filtros que não existem) e **não** servem como captura do sistema.

---

## 4. Dados necessários e citações não encontradas

- **Dado necessário:** medições de desempenho e disponibilidade para os RNF do cap. 5 (ver o `% TODO` na Subseção 10.8.2). Sem elas, o texto não afirma que os RNF foram atendidos.
- **Citações não encontradas:** nenhuma. Todas as afirmações que pediam respaldo receberam fonte lida. As que não precisavam (decisões de projeto) foram argumentadas a partir do código e do `DECISOES.md`, sem citação.

---

## 5. Pontos para conferir com a orientadora

- **Fontes sem data** (OWASP e MDN): ficaram sem `year`. O abnTeX2 deve exibir "[s.d.]". Confirmar se esse tipo de fonte (documentação técnica e guias da OWASP) é aceito.
- **Duas limitações de segurança** aparecem no cap. 10 (10.8.2) e são fáceis de corrigir no código, se houver tempo:
  1. imagens servidas em endereço público, sem checagem de privacidade (`/uploads` e R2);
  2. senha sem tamanho máximo (o bcrypt ignora o que passa de 72 bytes; a OWASP recomenda limitar).
  Se forem corrigidas antes da entrega, retirar os itens da lista de limitações.

---

## 6. Referências novas adicionadas ao `referencias.bib`

Todas têm o bloco `% VERIFICAR / Link / Localização / Acesso em / Observações` acima da entrada.

| Chave | Link | O que sustenta no cap. 10 | Grau de confiança |
|---|---|---|---|
| `fielding2022http` | https://www.rfc-editor.org/rfc/rfc9110.txt | Responder 404 em vez de 403 para ocultar recurso proibido (10.2.2) | Li o texto completo das Seções 15.5.4 e 15.5.5; DOI não consta no documento e ficou de fora |
| `atkins2022containment` | https://www.w3.org/TR/css-contain-3/ | `cqw` = 1% da largura do contêiner (10.3.1) | Li a Seção 6 completa; é Working Draft do W3C, não recomendação final |
| `campbell2024wcag` | https://www.w3.org/TR/WCAG22/ | Contraste mínimo 4,5:1 e fórmula (10.4.2); alvo de toque 24×24 (10.6.3) | Li os critérios 1.4.3 e 2.5.8 e as definições do glossário |
| `owasp2026password` | https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html | bcrypt custo ≥ 10 aceitável; Argon2id preferível; limite de 72 bytes (10.6.1 e 10.8.2) | Li o texto completo das seções citadas; sem autor nem data |
| `owasp2026forgot` | https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html | Mensagem neutra, token de uso único com expiração, invalidar sessões após recuperação (10.6.1) | Li o texto completo; sem autor nem data |
| `mdn2026canvas` | https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/canvas | `<canvas>` é bitmap e não é exposto a tecnologias assistivas (10.8.1) | Li a seção "Accessibility" completa; sem autor nem data |
| `owasp2026html5` | https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html | Não guardar identificador de sessão no `localStorage` (10.8.1) | Li a seção "Local Storage" completa; sem autor nem data |

Nenhuma chave já existente no `.bib` foi reutilizada.
