# Correções no sistema: segurança, deploy e requisitos

Levantamento de 02/10/2026, feito a partir do código de `Camila/TCC-backend` (commit `d43f62e`) e
`GitHub/TCC-frontend` (commit `9522918`, com alterações locais não commitadas) e da execução das
medições. Para cada problema: **onde está**, **por que importa**, **como corrigir**, **como conferir**
e **o que muda no TCC** depois de corrigido.

Nada aqui foi alterado no código: o documento só descreve. As sugestões de código são pontos de
partida; teste cada uma antes de publicar.

Prioridades:
- 🔴 **Bloqueia o deploy** ou quebra algo em produção: corrigir antes de publicar.
- 🟠 **Segurança / privacidade**: corrigir se houver tempo; senão, manter como limitação declarada.
- 🟡 **Requisito não atendido (RNF)**: melhora a nota da validação, mas não impede a publicação.
- ⚪ **Manutenção**: arrumação, sem efeito para o usuário.

---

## 🔴 1. `pnpm start:prod` não sobe a API

- **Onde:** `TCC-backend/package.json`, script `"start:prod": "node dist/main"`;
  `TCC-backend/tsconfig.json`, `"include": ["src/**/*", "prisma/**/*.ts"]`.
- **O que acontece:** como `prisma/seed.ts` entra na compilação, o `nest build` gera a saída em
  `dist/src/` e `dist/prisma/`. O arquivo `dist/main.js` não existe, e `node dist/main` falha com
  `MODULE_NOT_FOUND`. Isso foi constatado ao rodar as medições.
- **Como corrigir (escolha uma):**
  - (a) Tirar `prisma` da compilação de produção, em `tsconfig.build.json`:
    ```json
    "exclude": ["node_modules", "test", "dist", "**/*spec.ts", "prisma"]
    ```
    Apague a pasta `dist` antes de recompilar.
  - (b) Ou trocar o script: `"start:prod": "node dist/src/main"`.
- **Como conferir:** `rm -rf dist && pnpm build && pnpm start:prod`, e depois `GET /me` deve responder 401.
- **No TCC:** atualizar o 1º item da lista "ajustes antes da publicação" (Subseção Fluxo de
  Implantação) e o marcador de situação dos ajustes.

## 🔴 2. Limite de requisições vira global atrás do Caddy (`trust proxy`)

- **Onde:** `TCC-backend/src/main.ts` (não há `trust proxy`) e `src/app.module.ts:28-31` (limitador
  global de 100/min) + `auth.controller.ts:24` (*login*: 5/min) e `:35` (recuperação: 3 a cada 10 min).
- **O que acontece:** o limitador conta por IP. Atrás do Caddy, toda requisição chega à API vinda do
  próprio Caddy (127.0.0.1). Sem `trust proxy`, **todos os usuários dividem um contador**: cinco
  tentativas de *login* por minuto valem para a plataforma inteira, e 100 requisições por minuto
  somadas de todo mundo bastam para todos receberem 429.
- **Como corrigir:** em `main.ts`, informar ao Express que há exatamente um proxy confiável na frente:
  ```ts
  import { NestExpressApplication } from '@nestjs/platform-express';
  const app = await NestFactory.create<NestExpressApplication>(AppModule);
  app.set('trust proxy', 1); // um salto: o Caddy, que preenche X-Forwarded-For
  ```
  Use `1`, e não `true`. Com `true`, qualquer cliente poderia forjar `X-Forwarded-For` e escapar do limite.
- **Como conferir:** em produção, rodar o `latencia-api.mjs` de uma máquina e, ao mesmo tempo, abrir o
  site em outra rede. Nenhuma das duas pode receber 429 por causa da outra.
- **No TCC:** 2º item dos ajustes; a ressalva do RNF-03 cai se isso for corrigido.

## 🔴 3. Endereços diretos dão 404 na Vercel (`vercel.json`)

- **Onde:** falta `TCC-frontend/vercel.json`. As rotas são do `createBrowserRouter` (`src/rotas.tsx`).
- **O que acontece:** abrir ou recarregar `/categorias/...`, `/perfil/...`, o link do e-mail
  `/redefinir-senha?token=...` etc. pede um arquivo que não existe no servidor estático. A documentação
  da Vercel diz que *deep linking* não funciona sem configuração em SPA com Vite.
- **Como corrigir:** criar `vercel.json` na raiz do front (é o exemplo da documentação):
  ```json
  {
    "$schema": "https://openapi.vercel.sh/vercel.json",
    "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
  }
  ```
- **Como conferir:** depois do deploy, abrir direto uma URL interna e apertar F5 numa coleção.
- **No TCC:** 3º item dos ajustes.

## 🔴 4. Variáveis de produção e CORS

- **Onde:** `TCC-backend/src/main.ts:12-17`.
- **O que acontece:**
  - a API ainda autoriza `http://127.0.0.1:5500`, uma sobra do Live Server da fase em HTML puro;
  - o `FRONTEND_URL` precisa ser exatamente a URL da Vercel (com `https://` e sem barra no fim), senão
    o navegador bloqueia todas as chamadas;
  - o `VITE_API_URL` da Vercel precisa apontar para o domínio da API **com https**.
- **Como corrigir:** remover a linha `'http://127.0.0.1:5500',` e o comentário acima dela.
  Na Vercel, definir `VITE_API_URL` antes do *build* (ele é embutido na compilação; trocar depois
  exige um novo *deploy*).
- **Como conferir:** abrir o site publicado, fazer *login* e ver no DevTools (aba Rede) que não há
  erro de CORS.
- **No TCC:** 4º item dos ajustes.

## 🔴 5. E-mail de redefinição de senha não confirmado

- **Onde:** `TCC-backend/.env` (`SMTP_HOST` vazio nesta máquina); o CHECKLIST do front registra que o
  e-mail de 26/08/2026 não chegou.
- **O que acontece:** sem SMTP, o pedido de recuperação responde normalmente (mensagem neutra), mas
  nenhum e-mail sai; a falha só aparece no log (`auth.service.ts:101`).
- **Como corrigir:** configurar `SMTP_*` na VM com um provedor real e testar o fluxo inteiro até
  redefinir a senha.
- **No TCC:** o cap. 10 (Contas e Sessões) descreve o link de redefinição como funcionando. Se o
  e-mail não for configurado, é preciso dizer isso no texto.

## 🔴 6. Gerenciador de processo e reinício automático na VM

- **Onde:** não há nada no repositório (nem `ecosystem.config`, nem unidade do systemd).
- **O que acontece:** se o processo cair ou a VM reiniciar, a API fica fora do ar até alguém
  subir de novo. Isso afeta o RNF-08 (restrição de recuperação automática).
- **Como corrigir:** uma unidade do systemd com `Restart=always`, ou `pm2 start dist/main.js` (ou
  `dist/src/main.js`) com `pm2 startup`.
- **No TCC:** preencher o marcador do gerenciador de processo (Fluxo de Implantação).

---

## 🟠 7. Imagens de coleções privadas acessíveis pelo endereço

- **Onde:** `TCC-backend/src/app.module.ts:39-42` (`/uploads` servido sem autenticação) e
  `storage/r2-storage.service.ts:71` (o R2 devolve uma URL pública: `${publicUrl}/${chave}`).
- **O que acontece:** a privacidade é verificada nas rotas da API, mas a imagem é baixada direto do
  armazenamento. Quem tiver o endereço de uma imagem (por exemplo, quem viu a coleção antes de ela
  virar privada) continua conseguindo abri-la. O nome é um UUID, então não dá para adivinhar.
- **Como corrigir (do mais simples ao mais completo):**
  - (a) Manter e declarar: já está nas Limitações do cap. 10.
  - (b) Para o R2: tornar o balde privado e servir URLs assinadas com validade curta (o SDK S3 tem
    `getSignedUrl` em `@aws-sdk/s3-request-presigner`). A API gera a URL só depois de passar pela
    política de acesso. É a solução usual, porque a tag `<img>` não envia o cabeçalho `Authorization`.
  - (c) Uma rota da API que verifica a privacidade e devolve os bytes: simples de entender, mas
    coloca todo o tráfego de imagens na VM.
- **No TCC:** se corrigir, retirar o 1º item das Limitações e reescrever a 2ª observação da
  "Discussão e Implicações", que usa este caso como exemplo.

## 🟠 8. Senha sem tamanho máximo (bcrypt ignora além de 72 bytes)

- **Onde:** `TCC-backend/src/modules/user/dto/create-user.dto.ts:19`,
  `user/dto/change-password.dto.ts:8,14` e `auth/dto/reset-password.dto.ts:9,15` (só `@MinLength(8)`).
- **O que acontece:** o bcrypt só considera os primeiros 72 bytes. Uma senha longa tem o final
  ignorado: duas senhas que diferem só depois do 72º byte são aceitas como iguais. A OWASP recomenda
  limitar a senha a 72 bytes quando se usa bcrypt.
- **Como corrigir:** acrescentar um limite nos três DTOs. Para limitar por **bytes** (acentos e emoji
  ocupam mais de um byte em UTF-8), um validador customizado, por exemplo em
  `src/common/validators/senha-bcrypt.validator.ts`:
  ```ts
  import { ValidatorConstraint, ValidatorConstraintInterface } from 'class-validator';

  @ValidatorConstraint({ name: 'senhaAte72Bytes' })
  export class SenhaAte72Bytes implements ValidatorConstraintInterface {
    validate(valor: unknown) {
      return typeof valor === 'string' && Buffer.byteLength(valor, 'utf8') <= 72;
    }
    defaultMessage() {
      return 'A senha deve ter no máximo 72 bytes (cerca de 72 letras sem acento).';
    }
  }
  ```
  E, em cada campo de senha, `@Validate(SenhaAte72Bytes)` junto do `@MinLength(8)` (importar
  `Validate` de `class-validator`). Alternativa sem validador novo: `@MaxLength(64)`, que fica abaixo
  de 72 bytes para quase todo texto. Ajustar o campo da tela de cadastro com o mesmo limite.
- **Como conferir:** um teste unitário em `user.dto.spec.ts` com uma senha de 73 bytes deve ser recusado.
- **No TCC:** retirar o 2º item das Limitações (e a menção à OWASP nele, se fizer sentido).

## 🟠 9. Sugestões de etiquetas mostram palavras de coleções privadas

- **Onde:** `TCC-backend/src/modules/tag/service/tag.service.ts:28-34` (`findAll` sem filtro por dono
  ou privacidade). Foi uma decisão consciente (`DECISOES.md` §27).
- **O que acontece:** uma etiqueta usada só numa coleção privada ("terapia", por exemplo) aparece
  nas sugestões para qualquer pessoa. O conteúdo não vaza, mas a palavra sim.
- **Como corrigir:** filtrar as etiquetas pelas coleções que quem pergunta pode ver ou que são dele:
  ```ts
  where: {
    ...(query.search ? { name: { contains: query.search } } : {}),
    collections: { some: { OR: [
      { privacy: 'PUBLIC' },
      { category: { userId: viewerId } },
      { privacy: 'FRIENDS_ONLY', category: { userId: { in: friendIds } } },
    ] } },
  }
  ```
  `viewerId` vem do `@CurrentUser()` no controlador, e `friendIds` do
  `SocialAccessPolicyService.friendIdsOf`. Atenção: isso também esconde etiquetas que não estão em
  nenhuma coleção (as que sobraram depois de uma exclusão).
- **No TCC:** retirar o 3º item das Limitações.

## 🟠 10. Log de erros inesperados grava o objeto da exceção inteiro

- **Onde:** `TCC-backend/src/common/filters/global-exception.filter.ts:18`
  (`console.error(\`[${request.method}] ${request.url}\`, exception)`).
- **O que acontece:** para erros que não são `HttpException`, o log inclui a URL completa (com a
  *query string*) e o objeto do erro. Um erro do Prisma pode trazer valores de campos. Não se
  encontrou log de senha ou e-mail, mas o RNF-03 pede que dados sensíveis nunca apareçam em log.
- **Como corrigir:** registrar só o necessário:
  ```ts
  const { name, message, code } = exception as { name?: string; message?: string; code?: string };
  console.error(`[${request.method}] ${request.path}`, { name, code, message });
  ```
  `request.path` não inclui a *query string*. Se precisar da pilha para depurar, registre `stack` só
  fora de produção.

## 🟠 11. Token no `localStorage` (risco aceito)

- **Onde:** `TCC-frontend/src/lib/token-storage.ts`; decisão em `DECISOES.md` §3 do front.
- **O que acontece:** a OWASP recomenda não guardar identificador de sessão no `localStorage`, por
  ficar acessível a um eventual *script* malicioso (XSS). O projeto mitiga com o React escapando
  conteúdo, sem `innerHTML`, e com a sessão revogável no servidor.
- **Como reduzir o risco sem mudar a arquitetura:** publicar uma *Content Security Policy* na Vercel
  (bloco `headers` do `vercel.json`) que permita *scripts* só do próprio domínio. **Teste antes:** o
  `index.html` tem um *script* inline (o do tema), que a CSP bloquearia se não for liberado por *hash*.
- **No TCC:** nada muda; o cap. 10 já declara isso como contrapartida assumida.

## 🟠 12. Sem rota de verificação de saúde

- **Onde:** não existe. O README usa `GET /me` → 401 como teste.
- **Como corrigir:** uma rota pública que também testa o banco:
  ```ts
  @IsPublic()
  @Get('health')
  async health() {
    await this.prisma.$queryRaw`SELECT 1`;
    return { status: 'ok' };
  }
  ```
- **Por que vale a pena:** o monitor de disponibilidade (RNF-08) passa a detectar banco fora do ar,
  o que o `GET /me` não detecta.
- **No TCC:** se criar, trocar o parágrafo do Fluxo de Implantação que diz que a API não tem essa rota.

---

## 🟡 13. RNF-06: mover item espera o servidor (não atendido)

- **Onde:** `TCC-frontend/src/pages/Itens.tsx:176-188` (mutação sem `onMutate`).
- **Medido:** o melhor caso foi 568 ms (420 de gravação + 148 de nova leitura da lista), contra um
  limite de 500 ms.
- **Como corrigir:** atualização otimista do TanStack Query:
  - em `onMutate`, cancelar as consultas da lista (`cancelQueries`), guardar o estado anterior
    (`getQueryData`) e remover o item da lista com `setQueryData`;
  - em `onError`, devolver o estado guardado e mostrar o erro;
  - em `onSettled`, chamar `invalidateQueries` como hoje.
- **Como conferir:** medir com o `tela.js`/DevTools que o item some da lista sem esperar a rede.
- **No TCC:** o RNF-06 pode passar para "Atendido com ressalva" (o desfazer continua sendo por botão,
  não por Ctrl+Z), e o 3º parágrafo da "Discussão e Implicações" precisa ser revisto.

## 🟡 14. RNF-02: listas sem paginação e imagens sem otimização

- **Onde:** `item.service.ts:84-99`, `collection.service.ts:109-114` e `category.service.ts` (sem
  `take`/`skip`); upload sem redimensionamento (`DECISOES.md` §16 do back).
- **Como corrigir:**
  - paginação por cursor, como já existe em `notification.service.ts:100-118`, com `useInfiniteQuery`
    no front;
  - miniaturas com `sharp` no upload, preenchendo a coluna `thumbnail`, que já existe e está vazia
    de propósito.
- **No TCC:** sem as duas, o RNF-02 fica no máximo "Atendido com ressalva".

## 🟡 15. RNF-09: editor sem navegação por teclado

- **Onde:** `TCC-frontend/src/components/EditorDeFicha.tsx:443-445` (só `PointerSensor`);
  `DECISOES.md` §36 registra o corte.
- **Como reduzir:** o `@dnd-kit/core` tem `KeyboardSensor`; somado às setas para mover o elemento
  selecionado, já permite posicionar sem mouse. Os campos numéricos de posição já existem.
- **No TCC:** mesmo corrigido, o RNF-09 continua dependendo de uma auditoria (Lighthouse/axe e teste
  manual) para sair de "Não atendido".

## 🟡 16. RNF-10: sem registro de auditoria das exclusões

- **Onde:** só `item.service.ts:463` grava `EventLog`, e apenas para coleções não privadas.
- **Como corrigir:** uma tabela de auditoria (usuário, entidade, id, data) gravada na mesma transação
  das exclusões de categoria, coleção, item, layout e paleta. Também verificar o plano do Supabase:
  os *backups* diários com 30 dias de retenção dependem dele.

---

## ⚪ 17. Manutenção

- **Seed quebrado:** `prisma/seed.ts` cria usuário sem `userCode` (obrigatório e único) e com senha
  sem hash. Corrigir ou remover.
- **Teste e2e de modelo:** `test/app.e2e-spec.ts` importa `../src/modules/app.module` (não existe) e
  espera `GET /` → "Hello World!". Remover ou reescrever.
- **Nome do item:** banco `VarChar(255)` (`prisma/schema/collections.prisma:87`) e DTO
  `@MaxLength(200)` (`item/dto/create-item.dto.ts:14`). Escolher um só.
- **Aviso do driver `pg`:** o log da API mostrou `DeprecationWarning: Calling client.query() when the
  client is already executing a query is deprecated and will be removed in pg@9.0`. Não quebrou nada.
  Vale acompanhar ao atualizar `pg` ou o `@prisma/adapter-pg`.
- **README do back:** diz "39 suítes" (linha 470); hoje são 51.

---

## Ordem sugerida

1. Itens **1 a 6** antes de publicar (os quatro primeiros são pequenos).
2. Depois do deploy: rodar `scripts/medicoes` com `AMBIENTE=producao` e preencher os marcadores do cap. 10.
3. Se houver tempo: **8** (pequeno), **10** (pequeno), **12** (pequeno), **13** (médio), **9** (médio).
4. **7, 14, 15, 16**: maiores; se não der, ficam como limitações, que o TCC já declara.
