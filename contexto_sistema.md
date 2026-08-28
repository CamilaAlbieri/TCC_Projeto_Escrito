# Especificação do Sistema — Contexto para Desenvolvimento

> Documento de contexto gerado a partir do Capítulo 5 (Elicitação de Requisitos e Análise) do TCC.
> Objetivo: servir de referência para uma IA (ou pessoa) entender o domínio, os requisitos
> funcionais e não funcionais, e os casos de uso do sistema, a fim de apoiar a construção do
> front-end e do back-end. Não define stack, banco ou endpoints — apenas o comportamento esperado.

## Visão geral do produto

Plataforma personalizável para **criação e compartilhamento de coleções digitais**. O usuário
organiza seu acervo (livros, filmes, jogos, etc.) em uma hierarquia de três níveis e personaliza
a aparência das fichas de cada item por meio de um editor visual, no estilo de ferramentas como o Canva.

### Hierarquia de organização (3 níveis)

- **Categoria** — agrupador temático de alto nível (ex.: "Livros"). Não contém itens diretamente e não tem privacidade nem layout próprios.
- **Coleção** — subdivisão de uma categoria, onde os itens são efetivamente cadastrados (ex.: "Lidos", "Para Ler"). É o nível onde se configuram a **privacidade** e o **layout**.
- **Item** — unidade individual de conteúdo (ex.: o livro "Duna"). Pertence a uma coleção.

## Conceitos centrais do editor (Element, Layout, Item)

O editor funciona como o Canva: o usuário arrasta elementos para uma tela, redimensiona e posiciona.
A diferença é que a tela montada é um **modelo reutilizável** por vários itens.

- **Element** — Componente visual reutilizável utilizado para compor layouts, como textos, imagens, classificações, datas, listas, ícones, entre outros. Cada elemento possui tipo, posição, tamanho, conteúdo opcional e propriedades visuais próprias (fonte, tamanho da fonte, peso, alinhamento, cor e cor de fundo).
- **ElementType** — Conjunto fixo e predefinido de categorias de elemento que o usuário pode arrastar para o editor, à semelhança dos elementos básicos de ferramentas como o Canva. Compreende: texto fixo, campo de texto, imagem, gif, forma, ícone, classificação, data e lista. É representado por uma enumeração, pois constitui um conjunto estável que faz parte das capacidades do próprio editor.
- **Layout** — Estrutura personalizada que define quais elementos compõem a ficha de um item e como eles se organizam visualmente. Funciona como um _template_ reutilizável, associado a uma coleção; um mesmo layout pode ser reutilizado por diferentes coleções. Todos os itens de uma coleção compartilham o layout dela.
- **Item** — Unidade individual de conteúdo cadastrada pelo usuário (por exemplo, um livro, filme ou jogo específico). Todo item pertence a uma coleção e adota, por padrão, o layout associado a essa coleção, podendo receber uma versão visual própria conforme RF-17.

**Texto no canvas:** elementos textuais funcionam como objetos visuais editáveis, à semelhança do Canva. Um texto pode conter "Autor:" ou "Autor: Frank Herbert" diretamente em `Element.content`; se o usuário quiser separar rótulo e valor visualmente, pode criar dois elementos de texto independentes.

**Regra do layout:** o Layout é associado à **Coleção**. Todos os itens de uma coleção compartilham
o mesmo layout, e um mesmo layout pode ser reutilizado por coleções diferentes. Um item pode ter
_override_ individual (adicionar/remover elementos só para ele), sem afetar a coleção nem os demais itens.

**ElementType (tipos fixos de elemento):** texto fixo, campo de texto, imagem, gif, forma, ícone, classificação, data e lista.

## Atores

- **Usuário** — pessoa cadastrada; cria e gerencia categorias, coleções, itens, layouts e interações sociais.
- **Sistema** — executa ações automáticas (exibir fichas, enviar notificações, atualizar feed em tempo real).

## Grupos de requisitos funcionais

Os requisitos seguem esta ordem (do mais básico ao mais avançado):

1. **Acesso ao Sistema** — Cadastro, autenticação, recuperação e alteração de senha e gerenciamento do perfil do usuário.
2. **Coleção** — Gerenciamento de categorias, coleções, privacidade, itens e respectivas formas de visualização.
3. **Editor de Layout** — Criação, associação, compartilhamento e personalização de layouts e elementos.
4. **Aparência** — Configuração visual global da interface, incluindo tema, cores e tipografia.
5. **Interação Social** — Vínculos de amizade, acompanhamento de coleções e visualização de perfis de outros usuários.
6. **Notificação** — Geração, exibição e gerenciamento das notificações enviadas ao usuário.

## Índice dos requisitos funcionais

| Código | Título | Grupo | Prioridade | Ator |
|---|---|---|---|---|
| RF-01 | Cadastrar Usuário | Acesso ao Sistema | Essencial | Usuário |
| RF-02 | Autenticar Usuário (Login e Logout) | Acesso ao Sistema | Essencial | Usuário |
| RF-03 | Alterar Senha na Área Restrita | Acesso ao Sistema | Importante | Usuário |
| RF-04 | Recuperar Senha via E-mail | Acesso ao Sistema | Importante | Usuário |
| RF-05 | Gerenciar Perfil do Usuário | Acesso ao Sistema | Importante | Usuário |
| RF-06 | Gerenciar Categoria | Coleção | Essencial | Usuário |
| RF-07 | Gerenciar Coleção | Coleção | Essencial | Usuário |
| RF-08 | Gerenciar Privacidade da Coleção | Coleção | Essencial | Usuário |
| RF-09 | Gerenciar Item | Coleção | Essencial | Usuário |
| RF-10 | Mover Item entre Coleções | Coleção | Importante | Usuário |
| RF-11 | Exibir Ficha de Item em Modo Leitura | Coleção | Essencial | Sistema |
| RF-12 | Visualizar Itens de uma Coleção | Coleção | Essencial | Usuário |
| RF-13 | Exibir Tela Principal de Categorias | Coleção | Essencial | Sistema |
| RF-14 | Gerenciar Layout Personalizado | Editor de Layout | Importante | Usuário |
| RF-15 | Associar Layout a Coleção | Editor de Layout | Importante | Usuário |
| RF-16 | Compartilhar Layout | Editor de Layout | Desejável | Usuário |
| RF-17 | Modificar Layout de Item Individualmente | Editor de Layout | Desejável | Usuário |
| RF-18 | Gerenciar Aparência (Tema e Cores) | Aparência | Desejável | Usuário |
| RF-19 | Gerenciar Tipografia Global | Aparência | Desejável | Usuário |
| RF-20 | Gerenciar Amizade | Interação Social | Importante | Usuário |
| RF-21 | Seguir Coleção Visível | Interação Social | Importante | Usuário |
| RF-22 | Exibir Feed de Atualizações | Interação Social | Essencial | Sistema |
| RF-23 | Visualizar Perfis e Coleções de Usuários | Interação Social | Importante | Usuário |
| RF-24 | Gerenciar Notificações do Sistema | Notificação | Importante | Sistema |

## Requisitos Funcionais (detalhado)

### RF-01 — Cadastrar Usuário

- **Grupo:** Acesso ao Sistema
- **Ação:** Cadastrar
- **Objeto:** Usuário
- **Prioridade:** Essencial · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** nome (100 caracteres), email (150 caracteres), senha (mínimo 8 caracteres), foto de perfil (JPEG/PNG, máx. 5 MB), descrição (500 caracteres), código de usuário (gerado automaticamente pelo sistema, único e imutável).

**Exemplos:** Usuário preenche nome "Camila Albieri Mattos", email "camilamattos.mila@gmail.com", senha "Camila@2024" e conclui o cadastro com sucesso; o sistema gera automaticamente o código de usuário "camila#4827".
Usuário tenta cadastrar com o email "camilamattos.mila@gmail.com", que já existe, e recebe mensagem de erro informando que o email já está em uso.

**Regras / Restrições:**

1. Nome, email e senha são obrigatórios; foto de perfil e descrição são opcionais.
2. O email deve estar em formato válido (ex.: usuario@dominio.com).
3. Não podem ser registrados dois ou mais usuários com o mesmo email.
4. A senha deve ter no mínimo 8 caracteres.
5. A senha deve ser armazenada de forma criptografada (RNF-03).
6. O código de usuário é gerado automaticamente pelo sistema no momento do cadastro, deve ser único em toda a plataforma e não pode ser alterado posteriormente.
7. O código de usuário é o identificador público utilizado para buscas e envio de solicitações de amizade.
8. Ao concluir o cadastro, o sistema cria automaticamente para o usuário uma configuração de aparência padrão (tema claro, esquema neutro preto e branco), conforme RF-18.

### RF-02 — Autenticar Usuário (Login e Logout)

- **Grupo:** Acesso ao Sistema
- **Ação:** Autenticar (login e logout)
- **Objeto:** Usuário
- **Prioridade:** Essencial · **Operação:** Processamento · **Ator:** Usuário

**Atributos:** email (150 caracteres), senha (mínimo 8 caracteres), token de sessão (gerado automaticamente pelo sistema).

**Exemplos:** Usuário informa "camilamattos.mila@gmail.com" e "Camila@2024", o sistema valida as credenciais e concede acesso à área restrita.
Usuário informa email não cadastrado e recebe a mensagem "Credenciais de login inválidas".
Usuário clica em "Sair" e tem sua sessão encerrada imediatamente, sendo redirecionado para a tela de login.

**Regras / Restrições:**

1. Email e senha são obrigatórios para realizar o login.
2. O sistema deve verificar se o email está cadastrado e se a senha confere com o hash armazenado.
3. Não conferindo, o sistema deve exibir "Credenciais de login inválidas" sem especificar qual campo está errado.
4. A cada autenticação bem-sucedida o sistema registra uma sessão própria e a vincula ao token emitido.
5. O token de sessão tem prazo de validade definido, expirando automaticamente ao final desse prazo.
6. A validade da sessão é verificada a cada requisição autenticada, de modo que uma sessão revogada deixe de conceder acesso imediatamente, mesmo que o token ainda não tenha expirado.
7. O logout deve revogar imediatamente a sessão ativa, tornando o token inutilizável a partir desse momento.

### RF-03 — Alterar Senha na Área Restrita

- **Grupo:** Acesso ao Sistema
- **Ação:** Alterar senha
- **Objeto:** Usuário (autenticado)
- **Prioridade:** Importante · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** senha atual (para confirmação), nova senha (mínimo 8 caracteres), confirmação da nova senha.

**Exemplos:** Camila, já autenticada, acessa "Configurações de Conta", informa a senha atual, define a nova senha "NovaSenha@2025", confirma e o sistema atualiza a credencial.
Camila informa a senha atual incorreta e recebe a mensagem "Senha atual inválida".
Camila digita a nova senha e a confirmação divergentes e recebe aviso de que os campos não coincidem.

**Regras / Restrições:**

1. Esta operação só está disponível para o usuário autenticado, dentro da área restrita.
2. O usuário deve informar corretamente a senha atual antes de definir a nova senha.
3. A nova senha deve ter no mínimo 8 caracteres e ser diferente da senha atual.
4. A nova senha e sua confirmação devem coincidir.
5. A nova senha deve ser armazenada de forma criptografada (RNF-03).
6. Após a alteração, as demais sessões ativas do usuário devem ser invalidadas por segurança.

### RF-04 — Recuperar Senha via E-mail

- **Grupo:** Acesso ao Sistema
- **Ação:** Recuperar senha
- **Objeto:** Usuário (não autenticado)
- **Prioridade:** Importante · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** email cadastrado, token de redefinição (gerado automaticamente, único e temporário), nova senha (mínimo 8 caracteres), confirmação da nova senha.

**Exemplos:** Camila, sem estar logada, clica em "Esqueci minha senha", informa seu email e recebe uma mensagem com link de redefinição.
Camila abre o link recebido, define a nova senha e passa a conseguir autenticar-se com a nova credencial.
Camila tenta usar um link de redefinição expirado e recebe a mensagem de que o link não é mais válido, com opção de solicitar um novo.

**Regras / Restrições:**

1. A recuperação é iniciada por usuário não autenticado, a partir da tela de login.
2. O sistema solicita o email cadastrado e, se ele existir, envia um link de redefinição contendo um token único e temporário (RF-24).
3. Por segurança, a mensagem exibida ao solicitante deve ser a mesma independentemente de o email existir ou não, para não revelar quais emails estão cadastrados.
4. O token de redefinição deve expirar em no máximo 24 horas e ser de uso único.
5. Ao acessar o link, o usuário define a nova senha e sua confirmação, que devem coincidir e ter no mínimo 8 caracteres.
6. A nova senha deve ser armazenada de forma criptografada (RNF-03).
7. Concluída a redefinição, o token é invalidado e as sessões ativas anteriores são encerradas.

### RF-05 — Gerenciar Perfil do Usuário

- **Grupo:** Acesso ao Sistema
- **Ação:** Editar
- **Objeto:** Usuário
- **Prioridade:** Importante · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** nome de exibição (100 caracteres), email (150 caracteres), foto de perfil (JPEG/PNG, máx. 5 MB), descrição/bio (500 caracteres), token de verificação de e-mail (gerado automaticamente pelo sistema quando há alteração de e-mail).

**Exemplos:** Usuário acessa "Editar Perfil", altera o nome de exibição para "Cami Mattos", adiciona bio "Leitora e colecionadora de histórias" e salva as alterações.
Usuário altera o e-mail para "cami.mattos@gmail.com"; o sistema envia um link de verificação ao novo endereço e mantém o e-mail antigo ativo até a confirmação.

**Regras / Restrições:**

1. O nome de exibição deve ter entre 3 e 100 caracteres e é obrigatório.
2. A foto de perfil deve estar em formato JPEG ou PNG com tamanho máximo de 5 MB.
3. A descrição/bio é opcional e pode ser deixada em branco.
4. Alterações só têm efeito após o usuário salvar explicitamente.
5. Ao alterar o e-mail, o sistema deve enviar um link de verificação para o novo endereço antes de efetivar a mudança.
6. O e-mail antigo permanece ativo até que o novo seja verificado pelo usuário.
7. O token de verificação de e-mail deve expirar em até 24 horas após o envio.
8. Não é permitido alterar o e-mail para um endereço já cadastrado por outro usuário.
9. A alteração de senha não é feita por este requisito, e sim pelo RF-03 (área restrita).
10. O código de usuário (RF-01) não pode ser alterado por este caso de uso.

### RF-06 — Gerenciar Categoria

- **Grupo:** Coleção
- **Ação:** Gerenciar
- **Objeto:** Categoria (agrupador temático de alto nível que reúne coleções relacionadas)
- **Prioridade:** Essencial · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** nome (100 caracteres), tipo/tema (texto livre de até 50 caracteres, com "livros", "filmes", "séries", "jogos" e "músicas" oferecidos como sugestão; o usuário pode definir o seu próprio), ícone (emoji Unicode **ou** imagem JPEG/PNG de até 2 MB, opcional), cor de fundo do cartão (hexadecimal, opcional), descrição (300 caracteres), data de criação (gerada automaticamente).

**Exemplos:** Usuário Camila cria a categoria "Livros" com ícone personalizado e descrição "Tudo que leio ou pretendo ler"; a categoria é criada vazia, pronta para receber coleções.
Camila edita a categoria, renomeando para "Biblioteca Pessoal".
Camila exclui a categoria "Jogos Antigos" e confirma a exclusão permanente no diálogo de confirmação, que alerta sobre a remoção de todas as coleções e itens vinculados.

**Regras / Restrições:**

1. O nome da categoria é obrigatório; ícone e descrição são opcionais.
2. Não podem existir duas categorias com o mesmo nome para o mesmo usuário.
3. A categoria, ao ser criada, permanece vazia e não contém itens diretamente; os itens só podem ser cadastrados dentro de suas coleções (RF-07 e RF-09).
4. A categoria serve apenas como agrupador temático; a definição de layout ocorre no nível da coleção, e não no da categoria (RF-07 e RF-15).
5. A categoria não possui nível de privacidade próprio; a privacidade é configurada individualmente em cada coleção (RF-08).
6. A exclusão deve exibir diálogo de confirmação alertando que todas as coleções e itens vinculados serão removidos permanentemente.
7. A data de criação é gerada automaticamente pelo sistema e não pode ser editada.
8. O tema é gravado em letras minúsculas, sem espaços nas extremidades e sem acentuação, de modo que grafias diferentes da mesma palavra sejam tratadas como o mesmo tema. A exibição apresenta a primeira letra em maiúscula.
9. O ícone aceita uma única forma por vez: escolher uma imagem substitui o emoji, e escolher um emoji descarta a imagem.
10. Categoria sem ícone é exibida apenas com a cor de fundo, sem símbolo padrão.

### RF-07 — Gerenciar Coleção

- **Grupo:** Coleção
- **Ação:** Gerenciar
- **Objeto:** Coleção (subdivisão estruturada que pertence a uma categoria e agrupa itens)
- **Prioridade:** Essencial · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** nome (100 caracteres), categoria pai (referência obrigatória), ícone (emoji Unicode ou imagem JPEG/PNG de até 2 MB, opcional), cor de fundo do cartão (hexadecimal, opcional), descrição (300 caracteres), layout associado (referência opcional a um layout do usuário), nível de privacidade (pública, somente amigos ou privada), data de criação (gerada automaticamente).

**Exemplos:** Usuário cria a coleção "Lidos" dentro da categoria "Livros", definindo a privacidade como "pública"; a coleção passa a aceitar itens.
Camila edita a coleção "Lidos" para associar o layout "Card de Mangá", que passa a valer para os itens dessa coleção.
Camila exclui a coleção "Rascunhos" e confirma no diálogo a remoção de todos os itens vinculados.

**Regras / Restrições:**

1. O nome e a referência à categoria pai são obrigatórios.
2. Não podem existir duas coleções com o mesmo nome dentro da mesma categoria para o mesmo usuário.
3. Toda coleção pertence a exatamente uma categoria.
4. Por padrão, a privacidade de uma nova coleção é "pública", podendo ser alterada conforme RF-08.
5. Toda coleção pode ter um layout associado, que determina os elementos exibidos e preenchíveis nos itens dessa coleção; um mesmo layout pode ser reutilizado por diferentes coleções (RF-15).
6. A exclusão deve exibir diálogo de confirmação alertando que todos os itens vinculados serão removidos permanentemente.
7. A data de criação é gerada automaticamente pelo sistema e não pode ser editada.
8. O usuário deve poder alternar entre os modos de visualização "grade" e "lista" a qualquer momento; o modo padrão é "grade" e a escolha deve ser persistida entre sessões.
9. O usuário deve poder ordenar as coleções por pelo menos quatro critérios: nome crescente, nome decrescente, data de criação crescente e data de criação decrescente.
10. A ordenação padrão é por data de criação decrescente (mais recentes primeiro).
11. O usuário deve poder filtrar as coleções por nível de privacidade (RF-08); a ordenação e o filtro aplicados devem ser persistidos entre sessões.

### RF-08 — Gerenciar Privacidade da Coleção

- **Grupo:** Coleção
- **Ação:** Configurar
- **Objeto:** Nível de visibilidade da coleção
- **Prioridade:** Essencial · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** nível de privacidade (privada, somente amigos, pública), coleção alvo (referência).

**Exemplos:** Camila define a coleção "Lidos" como "pública", tornando-a visível a qualquer usuário.
Camila altera a coleção "Diário" para "privada"; ela deixa de aparecer para qualquer outro usuário.
Camila define "Favoritos" como "somente amigos"; apenas amigos confirmados passam a visualizá-la.

**Regras / Restrições:**

1. A privacidade é configurada no nível da coleção; a categoria não possui privacidade própria (RF-06).
2. Coleções "privadas" são visíveis unicamente pelo proprietário.
3. Coleções "somente amigos" liberam visualização e acompanhamento apenas para usuários com vínculo de amizade aceito (RF-20).
4. Coleções "públicas" ficam acessíveis a qualquer usuário cadastrado.
5. Por padrão, toda coleção recém-criada nasce como "pública" (RF-07).
6. Se uma coleção "pública" ou "somente amigos" for alterada para "privada", o sistema deve encerrar automaticamente o acompanhamento de todos os seguidores e remover seus eventos do feed (RF-21 e RF-22).
7. Se uma coleção "pública" for alterada para "somente amigos", o sistema deve manter o acompanhamento dos amigos confirmados e encerrar o dos demais seguidores (RF-20 e RF-21).

### RF-09 — Gerenciar Item

- **Grupo:** Coleção
- **Ação:** Gerenciar
- **Objeto:** Item dentro de uma coleção
- **Prioridade:** Essencial · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** nome (200 caracteres), coleção pai (referência obrigatória), cor de fundo do cartão (hexadecimal, opcional), layout visual efetivo do item, elementos visuais herdados ou personalizados (Texto, Campo de Texto, Imagem, GIF, Forma, Ícone, Classificação, Data e Lista), conteúdo e estilo armazenados nos próprios elementos, data de criação (gerada automaticamente).

**Exemplos:** Camila cria o item "Duna" na coleção "Lidos" (dentro da categoria "Livros"); o item abre com o layout aplicável e Camila edita visualmente os textos, imagens, datas, classificação e listas que aparecem na ficha, como faria em um canvas.
Camila adiciona individualmente um elemento de GIF e um texto extra "Personagem Favorito: Paul Atreides", sem afetar os demais itens nem os layouts da coleção e da categoria.
Posteriormente edita a classificação de 5 para 4 estrelas e, por fim, exclui o item "Duna — Rascunho" confirmando no diálogo de confirmação.

**Regras / Restrições:**

1. O nome do item é obrigatório; todos os demais campos são opcionais.
2. Todo item deve pertencer a uma coleção existente (RF-07); não é possível criar itens diretamente em uma categoria.
3. Ao ser criado, o item adota automaticamente a estrutura visual do layout associado à sua coleção (RF-15); caso a coleção não tenha layout, a ficha do item começa vazia e o próprio usuário monta sua composição (RF-17). A "visualização básica com campos padrão" é esse esqueleto montado pelo usuário, e não uma tela fixa oferecida pelo sistema.
4. O usuário pode adicionar, remover ou reposicionar elementos individualmente no item sem afetar os layouts da coleção ou da categoria, nem os demais itens (RF-17).
5. Não podem existir dois itens com o mesmo nome dentro da mesma coleção.
6. Imagens enviadas devem estar em formato JPEG ou PNG com tamanho máximo de 5 MB por arquivo.
7. A exclusão deve exibir diálogo de confirmação informando que a ação é irreversível.
8. A data de criação é gerada automaticamente e não pode ser editada pelo usuário.

### RF-10 — Mover Item entre Coleções

- **Grupo:** Coleção
- **Ação:** Mover
- **Objeto:** Item entre coleções, dentro da mesma categoria ou entre categorias distintas
- **Prioridade:** Importante · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** item de origem (nome, categoria e coleção atuais), destino (coleção existente, podendo pertencer a uma categoria diferente).

**Exemplos:** Camila arrasta "A Metamorfose" da coleção "Para Ler" para "Lidos", ambas na categoria "Livros"; como as duas coleções usam o mesmo layout, o item some da origem e aparece no destino imediatamente, sem mudança de estrutura.
Camila move "O Hobbit" da coleção "Lidos" para a coleção "Releituras Planejadas".
Camila move um item para uma coleção que usa um layout diferente e o sistema avisa que a estrutura de elementos pode mudar, exibindo prévia dos campos preservados.

**Regras / Restrições:**

1. Um item pode pertencer a apenas uma coleção por vez.
2. A movimentação deve ser concluída com no máximo três comandos (ex.: arrastar, soltar e confirmar).
3. Se a coleção de destino usa o mesmo layout da coleção de origem, todos os dados e a estrutura de elementos são preservados automaticamente.
4. Se o item ainda usa o layout da coleção, ao ser movido ele passa a seguir o layout da coleção de destino; se já possui composição visual própria, essa composição é preservada.
5. A operação deve ser reversível via ação de desfazer (Ctrl+Z ou botão equivalente) em até 30 segundos após a confirmação.
6. As contagens de itens das coleções de origem e destino devem ser atualizadas imediatamente.

### RF-11 — Exibir Ficha de Item em Modo Leitura

- **Grupo:** Coleção
- **Ação:** Exibir em modo leitura
- **Objeto:** Ficha individual de item
- **Prioridade:** Essencial · **Operação:** Saída · **Ator:** Sistema

**Atributos:** composição visual efetiva do item, elementos visuais com conteúdo e estilo, data de criação do item, modo de acesso (dono — com acesso à edição da composição; visitante — somente leitura).

**Exemplos:** Camila acessa a ficha do item "Duna" da coleção "Lidos" e visualiza todos os elementos visuais, com a opção de editar a composição disponível.
Um amigo visita a categoria pública "Livros" de Camila, abre a coleção "Lidos", acessa a ficha do item "Duna" e visualiza os mesmos elementos, porém sem nenhuma opção de edição.
Elementos não preenchidos continuam visíveis para visitantes com indicação visual de que estão vazios, sem permitir edição.

**Regras / Restrições:**

1. A ficha exibe os elementos da composição visual efetiva do item; quando existir personalização individual, ela prevalece sobre o layout da coleção.
2. Elementos sem conteúdo devem ser exibidos para dono e visitantes com indicação visual de que estão vazios; isso não concede permissão de edição ao visitante.
3. Para o dono do item, a ficha deve oferecer acesso direto à edição da **própria ficha**, isto é, da composição visual do item (RF-17). As ações de alterar os dados do item e de excluí-lo não ficam na ficha: elas pertencem ao menu de ações do item na listagem da coleção, de onde valem para qualquer item sem precisar abri-lo.
4. Para visitantes, a ficha é estritamente somente leitura, sem nenhuma opção de edição visível.
5. A ficha deve respeitar a aparência (tema e cores) configurada pelo dono (RF-18).
6. O sistema deve fornecer ao front-end as informações necessárias para exibir a ficha final do item em modo leitura, diferenciando visualmente o acesso do dono e de visitantes.

### RF-12 — Visualizar Itens de uma Coleção

- **Grupo:** Coleção
- **Ação:** Visualizar
- **Objeto:** Itens de uma coleção
- **Prioridade:** Essencial · **Operação:** Saída · **Ator:** Usuário

**Atributos:** lista de itens da coleção selecionada, critérios de ordenação (nome A–Z, nome Z–A, data de criação crescente, data de criação decrescente, avaliação crescente, avaliação decrescente), critérios de filtro (conteúdo dos elementos visuais e campos personalizados do layout aplicável).

**Exemplos:** Camila abre a coleção "Lidos" da categoria "Livros" e visualiza todos os 30 itens ordenados por data de criação decrescente.
Camila ordena por avaliação decrescente e filtra por gênero "Ficção Científica", vendo apenas os livros desse gênero do melhor ao pior avaliado.
Camila visualiza a coleção "Favoritos" usando o mesmo filtro; o sistema mantém a preferência de ordenação, mas os filtros são independentes por coleção.

**Regras / Restrições:**

1. A visualização deve exibir todos os itens da coleção selecionada.
2. A ordenação padrão é por data de criação decrescente (mais recentes primeiro).
3. Os critérios de filtro disponíveis dependem dos elementos presentes no layout aplicável à coleção.
4. A ordenação aplicada deve ser persistida por usuário e válida para todas as coleções até ser alterada.
5. Os filtros aplicados são mantidos durante a navegação dentro da mesma coleção e reiniciados ao sair dela.

### RF-13 — Exibir Tela Principal de Categorias

- **Grupo:** Coleção
- **Ação:** Exibir
- **Objeto:** Tela principal (dashboard) de categorias do usuário
- **Prioridade:** Essencial · **Operação:** Saída · **Ator:** Sistema

**Atributos:** lista de categorias do usuário autenticado (nome, ícone, cor de fundo do cartão, contagem total de coleções, contagem total de itens somados entre todas as coleções), modo de exibição (grade ou lista), critérios de ordenação (nome A–Z, nome Z–A, data de criação crescente, data de criação decrescente), critérios de filtro (tipo/tema da categoria), atalhos de navegação para coleções e itens.

**Exemplos:** Ao fazer login, Camila vê na tela inicial os cartões "Livros (2 coleções, 20 itens)", "Filmes (2 coleções, 30 itens)" e "Cafés (1 coleção, 7 itens)" em modo grade.
Camila clica em "Livros" e visualiza as coleções "Lidos (20, pública)", "Para Ler (30, somente amigos)" e "Diário (2, privada)", podendo navegar para os itens de cada uma.
Camila alterna para o modo lista e ordena as categorias por data de criação decrescente; a preferência é mantida na próxima sessão.
Camila, em seu primeiro acesso, vê mensagem de boas-vindas e botão "Criar primeira categoria".

**Regras / Restrições:**

1. A tela deve exibir todas as categorias do usuário autenticado em uma única tela dedicada.
2. Cada categoria deve exibir nome, ícone e as contagens de coleções e de itens.
3. A categoria não possui nível de privacidade próprio; o indicador individual de privacidade aparece no card de cada coleção dentro da categoria, e não no card da categoria (RF-08).
4. A navegação até as coleções de uma categoria deve ser acessível com um único clique ou toque; a navegação até um item, com no máximo três.
5. Usuários sem categorias devem ver mensagem de boas-vindas com atalho para criação da primeira categoria.
6. Ao acessar uma categoria vazia (sem coleções), o sistema deve exibir mensagem informativa e atalho para criação da primeira coleção.
7. O usuário deve poder alternar entre os modos de visualização "grade" e "lista" a qualquer momento.
8. O modo de visualização padrão é "grade"; a escolha do usuário deve ser persistida entre sessões.
9. O usuário deve poder ordenar as categorias por pelo menos quatro critérios: nome crescente, nome decrescente, data de criação crescente e data de criação decrescente.
10. A ordenação padrão é por data de criação decrescente (mais recentes primeiro).
11. Os filtros e a ordenação aplicados devem ser persistidos entre sessões do mesmo usuário.
12. O filtro por tema opera sobre o texto livre definido pelo usuário (RF-06), comparando os valores já normalizados, de modo que grafias diferentes da mesma palavra sejam tratadas como o mesmo tema.

### RF-14 — Gerenciar Layout Personalizado

- **Grupo:** Editor de Layout
- **Ação:** Gerenciar e duplicar
- **Objeto:** Layout personalizado (template)
- **Prioridade:** Importante · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** nome do layout (100 caracteres), elementos disponíveis (conforme os tipos fixos definidos pela enumeração ElementType: texto fixo, campo de texto, imagem, gif, forma, ícone, classificação, data e lista), conteúdo textual opcional do elemento, propriedades visuais do elemento (fonte, tamanho, peso, estilo, alinhamento, cor e cor de fundo), posição e tamanho de cada elemento na área de edição, layout de origem (referência opcional, preenchida apenas na operação de duplicação).

**Exemplos:** Camila cria o layout "Card de Livro" com elemento de Imagem para capa, textos livres como "Autor: Frank Herbert", Classificação de 1 a 5 estrelas, dois elementos de Data (início e fim de leitura) e área de Texto para notas.
Camila reposiciona o elemento de Classificação via _drag-and-drop_ e visualiza o resultado em tempo real na prévia.
Camila seleciona o layout "Card de Livro" e clica em "Duplicar"; o sistema cria "Card de Livro (cópia)" com a mesma estrutura, que Camila renomeia para "Card de Livro — Edição Especial" e ajusta sem afetar o original.
Camila exclui o layout "Card Antigo" que não está associado a nenhuma coleção.

**Regras / Restrições:**

1. O nome do layout é obrigatório e deve ser único por usuário.
2. Pelo menos um elemento deve ser adicionado para que o layout possa ser salvo.
3. Os tipos de elemento disponíveis para inclusão são os definidos pela enumeração ElementType (texto fixo, campo de texto, imagem, gif, forma, ícone, classificação, data e lista), à semelhança dos elementos fixos oferecidos por editores visuais como o Canva.
4. Elementos podem armazenar conteúdo visual próprio em `content`, como "Autor:" ou "Autor: Frank Herbert", além de propriedades de fonte, cor e alinhamento.
5. Elementos podem ser reposicionados e redimensionados via arrastar e soltar (_drag-and-drop_).
6. Alterações devem ser refletidas em tempo real na prévia do editor, sem recarregar a página.
7. A duplicação deve copiar integralmente a estrutura de elementos (tipo, conteúdo, estilo, posição, tamanho e propriedades) do layout de origem.
8. Na duplicação, o sistema sugere automaticamente o nome original acrescido de "(cópia)", podendo ser editado antes de salvar.
9. Alterações no layout duplicado não devem refletir no layout de origem.
10. O usuário pode excluir um layout somente se ele não estiver associado a nenhuma coleção ativa (RF-15).
11. A data de criação do layout duplicado é a data da operação, e não a do layout de origem.

### RF-15 — Associar Layout a Coleção

- **Grupo:** Editor de Layout
- **Ação:** Associar
- **Objeto:** Layout (template) a uma coleção
- **Prioridade:** Importante · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** coleção de destino (existente, pertencente ao usuário), layout selecionado (pertencente ao usuário, podendo já estar associado a outras coleções).

**Exemplos:** Camila associa o layout "Card de Livro" à coleção "Lidos"; todos os itens dessa coleção passam a usar esse layout.
Camila associa o mesmo layout "Card de Livro" também à coleção "Para Ler"; as duas coleções reutilizam o mesmo template, de forma independente.
Camila troca o layout da coleção "Mangás" para "Card de Mangá"; os novos itens passam a usar o novo layout, e os itens já existentes mantêm seus dados (RF-17).

**Regras / Restrições:**

1. Um layout só pode ser associado a coleções existentes e pertencentes ao mesmo usuário.
2. O layout é associado no nível da coleção; a categoria não recebe layout (RF-06).
3. Uma coleção pode ter, no máximo, um layout ativo por vez.
4. Um mesmo layout pode ser associado a várias coleções simultaneamente, sendo reutilizado como template.
5. O layout aplicável a um item é o da sua coleção; na ausência de layout associado, o item pode receber uma composição própria, montada na ficha. Modificações individuais por item são tratadas como _override_ (RF-17).
6. A troca ou remoção do layout da coleção não altera os dados nem os elementos individuais (_override_) dos itens já cadastrados (RF-17).
7. Ao associar um layout, o sistema deve exibir prévia de como os novos itens serão criados.
8. O usuário pode remover a associação de layout da coleção a qualquer momento.

### RF-16 — Compartilhar Layout

- **Grupo:** Editor de Layout
- **Ação:** Compartilhar e importar
- **Objeto:** Layout (template) compartilhado entre usuários
- **Prioridade:** Desejável · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** layout de origem (pertencente ao usuário que compartilha), identificador do layout compartilhado (`idLayout`, usado em link ou payload de importação), usuário importador, layout importado (cópia gerada na biblioteca do importador), finalidade interna do layout (`IMPORTED_TEMPLATE`) para indicar que a cópia nasceu de uma importação.

**Exemplos:** Camila cria o layout "Card de Livro" e envia para uma amiga um link contendo o identificador desse layout.
A amiga importa o layout pela biblioteca de layouts; o sistema cria uma cópia na conta dela, que pode ser usada e editada livremente como se tivesse sido criada por ela.
Camila altera o layout original depois do compartilhamento; as cópias já importadas por outros usuários permanecem intactas.

**Regras / Restrições:**

1. O compartilhamento simples usa o próprio identificador do layout (`idLayout`) em link ou payload de importação; não há código separado, ativação ou cancelamento de compartilhamento neste fluxo.
2. A importação cria uma cópia independente do layout na biblioteca do usuário importador; alterações na cópia não afetam o layout de origem, e vice-versa.
3. Layouts importados devem aparecer na biblioteca do usuário como templates utilizáveis, mas ficam marcados internamente com a finalidade `IMPORTED_TEMPLATE`.
4. O compartilhamento abrange apenas a estrutura visual do layout, não os itens do dono nem as personalizações individuais feitas em itens.
5. A importação está disponível a partir do identificador/link de compartilhamento, independentemente de haver vínculo de amizade entre os usuários.
6. O usuário não deve importar um layout da própria biblioteca por este fluxo; para isso deve usar a duplicação.

### RF-17 — Modificar Layout de Item Individualmente

- **Grupo:** Editor de Layout
- **Ação:** Modificar individualmente
- **Objeto:** Layout herdado em um item específico
- **Prioridade:** Desejável · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** elementos herdados do layout aplicável ao item (base de partida), conteúdo e estilo dos elementos, elementos adicionados pelo usuário no item (_override_), elementos removidos pelo usuário no item (_override_), elementos reposicionados ou redimensionados no item (_override_).

**Exemplos:** Camila cria o item "Harry Potter e a Pedra Filosofal" na coleção "Lidos" (categoria "Livros"); o item abre com o layout aplicável herdado; Camila adiciona elemento extra de texto "Data de Início de Leitura" apenas nesse item.
Camila remove o elemento de GIF de um item específico pois não se aplica, sem afetar o layout da coleção, o da categoria nem os outros itens.
Depois da personalização, o item mantém sua própria composição visual, sem alterar o layout usado pela coleção.

**Regras / Restrições:**

1. Todo item, ao ser criado, adota o layout da sua coleção como ponto de partida (RF-09 e RF-15). Caso a coleção não possua layout, o item começa sem composição, e o usuário pode montá-la a partir da própria ficha.
2. O usuário pode adicionar, remover ou reposicionar elementos no item sem afetar o layout da coleção, o da categoria ou os demais itens.
3. O layout aplicável deve ser exibido como referência visual durante a edição individual do item.
4. As modificações individuais são exclusivas do item; nenhuma alteração feita no item reflete nos layouts da coleção ou da categoria.
5. Ao iniciar a edição visual individual, o sistema deve criar uma cópia própria do layout da coleção para aquele item, preservando elementos, conteúdo e estilos como ponto de partida. Não havendo layout na coleção, a cópia é criada vazia: a existência de um layout na coleção não é pré-requisito para personalizar a ficha de um item.
6. Após a criação da cópia individual, alterações de conteúdo, estilo, posição, tamanho, adição ou remoção de elementos devem afetar apenas aquele item, sem modificar o layout da coleção nem os demais itens.
7. Depois de personalizado, o item mantém sua composição visual própria, mesmo que o layout da coleção seja alterado posteriormente.

### RF-18 — Gerenciar Aparência (Tema e Cores)

- **Grupo:** Aparência
- **Ação:** Gerenciar aparência
- **Objeto:** Configuração visual global da interface do usuário
- **Prioridade:** Desejável · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** cor primária (hex), subcores (até 4 valores hex), paletas predefinidas (lista do sistema), paletas personalizadas (nome de até 50 caracteres), estilo global (claro / escuro / automático conforme sistema operacional).

**Exemplos:** No momento do cadastro (RF-01), o sistema cria para o usuário uma aparência padrão: tema claro, com esquema neutro em preto e branco.
Camila seleciona a paleta predefinida "Outono" com tons terrosos e estilo escuro; toda a interface é atualizada em tempo real.
Camila cria a paleta personalizada "Meu Rosa" com cor primária #E91E8C, aplica e salva no perfil.

**Regras / Restrições:**

1. Todo usuário recém-cadastrado recebe uma configuração de aparência padrão (tema claro, esquema neutro preto e branco), criada automaticamente no cadastro (RF-01).
2. O usuário pode alterar a aparência a qualquer momento; as mudanças sobrescrevem a configuração existente.
3. O tema selecionado deve ser aplicado globalmente a todas as telas do sistema de forma imediata.
4. Paletas predefinidas podem ser editadas diretamente nas configurações.
5. As configurações de aparência devem ser salvas no perfil e persistidas entre sessões e dispositivos.
6. As alterações devem ser exibidas em área de prévia antes de o usuário confirmar a aplicação.

### RF-19 — Gerenciar Tipografia Global

- **Grupo:** Aparência
- **Ação:** Alterar
- **Objeto:** Tipografia e aparência textual global
- **Prioridade:** Desejável · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** família de fonte (lista predefinida com no mínimo 5 opções), tamanho base em px (mínimo 12 px, máximo 24 px), espaço entre linhas (1.0 a 2.0), espaço entre letras.

**Exemplos:** Camila altera a fonte para "Georgia", tamanho base para 16 px e espaço de linha para 1.6; a prévia reflete as mudanças antes da confirmação.

**Regras / Restrições:**

1. As fontes disponíveis devem incluir ao menos cinco opções tipográficas distintas (ex.: serif, sans-serif, monospace).
2. As alterações devem ser exibidas em área de prévia antes de o usuário confirmar a aplicação.
3. As configurações de tipografia devem ser aplicadas globalmente e persistidas entre sessões.

### RF-20 — Gerenciar Amizade

- **Grupo:** Interação Social
- **Ação:** Adicionar, aceitar e visualizar
- **Objeto:** Amizade entre usuários
- **Prioridade:** Importante · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** usuário solicitante (nome, código de usuário e email), usuário receptor (nome, código de usuário e email), critério de busca (código de usuário ou nome), status da solicitação (pendente / aceita / recusada), data da solicitação.

**Exemplos:** Camila informa o código de usuário "joao#1203" no campo de busca e envia a solicitação de amizade; João aceita e ambos aparecem mutuamente na lista de amigos.
Camila busca por "João Silva" pelo nome e o sistema exibe a lista de usuários correspondentes; Camila seleciona o usuário correto e envia a solicitação.
Camila recusa a solicitação de um usuário desconhecido; o status é atualizado para "recusada" sem gerar notificação ao solicitante.

**Regras / Restrições:**

1. A busca por usuários pode ser realizada tanto pelo código de usuário (busca exata) quanto pelo nome (busca parcial).
2. A busca por código de usuário retorna no máximo um resultado, pois o código é único.
3. A busca por nome pode retornar múltiplos resultados; o sistema exibe nome, foto e código de cada usuário para desambiguação.
4. Não é possível enviar solicitação de amizade para si mesmo.
5. Uma solicitação pendente não pode ser reenviada enquanto aguarda resposta.
6. A lista de amigos exibe apenas amizades com status "aceita".
7. O usuário pode desfazer uma amizade aceita a qualquer momento, removendo o vínculo para ambos os lados.
8. Envio, aceite e desfazer amizade devem gerar notificação ao usuário envolvido, conforme RF-24; a recusa é silenciosa.

### RF-21 — Seguir Coleção Visível

> **Decisão de produto — 28/06/2026:** o acompanhamento de coleções públicas deixou de exigir amizade. Amizade continua obrigatória para coleções `FRIENDS_ONLY`. A mudança separa o acesso social da assinatura de atualizações.

- **Grupo:** Interação Social
- **Ação:** Seguir
- **Objeto:** Coleção visível de outro usuário
- **Prioridade:** Importante · **Operação:** Entrada · **Ator:** Usuário

**Atributos:** coleção seguida (com privacidade pública ou "somente amigos"), categoria pai da coleção (referência), usuário seguidor, data de início do acompanhamento.

**Exemplos:** Camila visita o perfil de "João Silva", com quem não possui amizade, vê a coleção pública "Filmes Assistidos" (dentro da categoria "Filmes") e clica em "Seguir"; as atualizações dessa coleção passam a aparecer no feed de Camila, mas as demais coleções da categoria não, a menos que sejam seguidas individualmente.
Após João aceitar a amizade de Camila, ela também passa a visualizar e pode seguir a coleção "Favoritos", configurada como "somente amigos".
Camila tenta acessar uma coleção privada de João e não encontra a opção de acompanhamento, pois a coleção sequer aparece no perfil visitado.

**Regras / Restrições:**

1. Qualquer usuário autenticado pode seguir uma coleção "pública", independentemente de amizade com o proprietário.
2. Coleções "somente amigos" podem ser seguidas apenas enquanto existir vínculo de amizade confirmado (RF-20).
3. Coleções "privadas" não aparecem e não podem ser seguidas por outros usuários, nem mesmo por amigos.
4. O proprietário não pode seguir a própria coleção.
5. O acompanhamento ocorre no nível da coleção: o usuário pode seguir múltiplas coleções da mesma categoria, de forma independente, sem que isso implique seguir as demais.
6. O usuário pode deixar de seguir uma coleção a qualquer momento, sem restrições.
7. Se a coleção seguida for alterada para "privada", todos os acompanhamentos devem ser encerrados automaticamente (RF-08).
8. Se uma coleção "pública" for alterada para "somente amigos", acompanhamentos de usuários sem amizade confirmada devem ser encerrados; os de amigos permanecem ativos.
9. Ao desfazer uma amizade, acompanhamentos de coleções "públicas" permanecem ativos e acompanhamentos de coleções "somente amigos" entre os dois usuários são encerrados.

### RF-22 — Exibir Feed de Atualizações

- **Grupo:** Interação Social
- **Ação:** Exibir
- **Objeto:** Feed de atualizações das coleções seguidas
- **Prioridade:** Essencial · **Operação:** Saída · **Ator:** Sistema

**Atributos:** tipo de evento (adição, edição ou remoção de item em coleção seguida), item afetado ou snapshot mínimo do item quando ele já tiver sido removido, coleção seguida, categoria pai, usuário autor da ação, data e hora do evento e descrição textual auxiliar.

**Exemplos:** Feed de Camila exibe "João adicionou Oppenheimer na coleção Filmes Assistidos (categoria Filmes) — há 2 horas".
Feed exibe "Ana avaliou o item Breaking Bad na coleção Séries Concluídas — há 5 minutos".
Camila rola o feed e carrega eventos mais antigos progressivamente.

**Regras / Restrições:**

1. O feed deve exibir apenas eventos de coleções que o usuário segue ativamente (RF-21).
2. Os eventos devem ser ordenados cronologicamente, do mais recente ao mais antigo.
3. O feed deve ser atualizado automaticamente em tempo real, sem necessidade de recarregar a página.
4. Se a privacidade de uma coleção seguida for alterada para "privada", os eventos anteriores dessa coleção não devem mais ser exibidos, e o acompanhamento é encerrado automaticamente (RF-21).
5. Eventos de criação ou exclusão de coleção em si não são exibidos no feed; a descoberta de novas coleções ocorre por meio da visita a perfis de usuários (RF-23).
6. O feed deve suportar carregamento progressivo (paginação) para históricos longos.
7. O registro de evento deve usar campos estruturados (`collectionId`, `itemId` opcional e `userId` autor) como fonte principal dos dados; `description` é apenas texto auxiliar.
8. Eventos de remoção de item devem preservar snapshot mínimo do item afetado para que o histórico continue compreensível após a exclusão física do item.

### RF-23 — Visualizar Perfis e Coleções de Usuários

- **Grupo:** Interação Social
- **Ação:** Visualizar
- **Objeto:** Perfil, categorias, coleções visíveis e itens de outro usuário
- **Prioridade:** Importante · **Operação:** Saída · **Ator:** Usuário

**Atributos:** perfil do usuário (nome, foto, bio), coleções acessíveis (públicas ou "somente amigos", conforme o vínculo), categorias agrupadoras dessas coleções, itens pertencentes a cada coleção visível.

**Exemplos:** Camila visita o perfil de "João Silva" sem possuir amizade e visualiza as coleções públicas de João agrupadas pelas respectivas categorias. Após a amizade ser aceita, ela também passa a visualizar "Filmes / Favoritos", configurada como "somente amigos"; dentro de cada coleção visível, vê os itens cadastrados.
Camila não encontra a coleção "Diário Pessoal" de João (categoria "Diários"), pois ela é privada; como é a única coleção da categoria, a categoria "Diários" inteira não aparece para Camila.
A categoria "Jogos" de João aparece para Camila apenas com as coleções não-privadas; coleções privadas dessa mesma categoria continuam ocultas.

**Regras / Restrições:**

1. Apenas coleções com privacidade "pública" ou "somente amigos" são exibidas no perfil visitado, respeitando o vínculo de amizade entre os usuários (coleções "somente amigos" só aparecem para amigos confirmados).
2. Coleções "privadas" e seus itens não devem ser listados nem acessíveis, mesmo que o amigo compartilhe um link direto.
3. Categorias servem apenas como agrupadores visuais no perfil visitado e não possuem privacidade própria.
4. Categorias que, para o visitante, não tenham nenhuma coleção visível devem ser ocultadas integralmente do perfil, de modo que o visitante não tenha conhecimento de sua existência.
5. Todos os itens dentro de uma coleção visível são acessíveis ao visitante; não há privacidade individual por item.
6. A navegação nas categorias, coleções e itens de outros usuários é somente leitura; não é possível editar ou excluir dados.

### RF-24 — Gerenciar Notificações do Sistema

- **Grupo:** Notificação
- **Ação:** Gerar, exibir e gerenciar
- **Objeto:** Notificações do sistema
- **Prioridade:** Importante · **Operação:** Saída · **Ator:** Sistema

**Atributos:** tipo de notificação (solicitação de amizade recebida, solicitação de amizade aceita, amizade desfeita, atualização em coleção seguida, verificação de e-mail, recuperação de senha), usuário destinatário, usuário autor (quando aplicável), mensagem (texto descritivo gerado pelo sistema), evento estruturado vinculado quando for atualização de coleção seguida (`eventLog` com coleção, categoria, item e snapshot), data e hora de geração, status (não lida / lida), data e hora de leitura quando aplicável, canal de entrega (in-app, sempre; e-mail, para verificação de e-mail e recuperação de senha).

**Exemplos:** João envia solicitação de amizade para Camila; o sistema gera notificação in-app para Camila com o texto "João Silva (joao#1203) enviou uma solicitação de amizade" e exibe indicador visual de notificação não lida.
Camila aceita a solicitação; o sistema gera notificação in-app para João com o texto "Camila Mattos aceitou sua solicitação de amizade".
Camila solicita recuperação de senha; o sistema envia notificação via e-mail com link de redefinição (conforme RF-04).

**Regras / Restrições:**

1. Toda solicitação de amizade enviada ou aceita deve gerar notificação in-app ao usuário envolvido; recusas são silenciosas (RF-20).
2. As notificações in-app devem ser exibidas em uma área dedicada (ex.: ícone de sino no menu principal) com indicador visual de quantidade de notificações não lidas.
3. As notificações devem ser ordenadas cronologicamente, da mais recente à mais antiga.
4. O usuário pode marcar uma notificação como lida, marcar todas como lidas ou limpar sua área de notificações sem excluir os registros do sistema; `isRead` indica o estado atual, `readAt` registra quando a leitura ocorreu e a limpeza oculta as notificações da listagem do usuário.
5. Notificações de verificação de e-mail (RF-05) e de recuperação de senha (RF-04) devem ser enviadas também pelo canal de e-mail, contendo link único e com prazo de expiração.
6. Notificações referentes a coleções cuja privacidade tenha sido alterada para "privada" devem ser ocultadas automaticamente do destinatário (RF-08).
7. As notificações in-app serão persistidas para consulta pela API; atualização em tempo real por SSE permanece fora desta etapa.
8. Notificações lidas devem ser mantidas por no mínimo 30 dias antes de qualquer limpeza automática.

## Requisitos Não-Funcionais

| Código | Categoria | Prioridade |
|---|---|---|
| RNF-01 | Usabilidade | Essencial |
| RNF-02 | Desempenho | Essencial |
| RNF-03 | Segurança | Essencial |
| RNF-04 | Compatibilidade | Essencial |
| RNF-05 | Personalização | Importante |
| RNF-06 | Desempenho | Importante |
| RNF-07 | Desempenho | Essencial |
| RNF-08 | Disponibilidade | Importante |
| RNF-09 | Acessibilidade | Importante |
| RNF-10 | Segurança | Essencial |

### RNF-01 — Usabilidade

- **Categoria:** Usabilidade · **Prioridade:** Essencial

**Descrição:** A interface deve ser intuitiva, facilitando o uso do editor de layout e das funcionalidades de personalização visual.

**Métrica:** Usuários iniciantes devem ser capazes de criar uma coleção e adicionar um item em até 5 minutos sem consultar documentação externa.

**Restrições:**

1. Todas as ações principais devem ser acessíveis em no máximo três cliques a partir da tela inicial.
2. Elementos interativos devem possuir rótulos descritivos e tooltips explicativos.
3. Mensagens de erro devem ser claras, indicando o problema e a ação corretiva.
4. O sistema deve utilizar ícones e terminologia consistentes em todas as telas.

### RNF-02 — Desempenho de Carregamento

- **Categoria:** Desempenho · **Prioridade:** Essencial

**Descrição:** O sistema deve carregar categorias, coleções e itens em tempo adequado sob condições normais de uso.

**Métrica:** O tempo de carregamento de qualquer tela principal (categorias, coleções, lista de itens) não deve exceder 3 segundos em conexão de banda larga padrão (10 Mbps).

**Restrições:**

1. Imagens de capa e perfil devem ser otimizadas automaticamente pelo sistema para reduzir o tempo de carregamento.
2. Listas com mais de 20 itens devem utilizar carregamento progressivo (paginação ou scroll infinito).
3. Requisições à API devem retornar resposta em no máximo 2 segundos para operações de leitura.
4. Operações de escrita (cadastro, edição, exclusão) devem ser concluídas em no máximo 3 segundos.

### RNF-03 — Segurança de Senhas e Dados Pessoais

- **Categoria:** Segurança · **Prioridade:** Essencial

**Descrição:** O sistema deve criptografar senhas e proteger informações pessoais dos usuários de acordo com boas práticas de segurança.

**Métrica:** Todas as senhas devem ser armazenadas utilizando algoritmo de hash seguro (bcrypt ou argon2) com salt único por usuário.

**Restrições:**

1. Senhas nunca devem ser armazenadas em texto puro ou com criptografia reversível.
2. Toda comunicação entre cliente e servidor deve utilizar protocolo HTTPS/TLS.
3. Tokens de sessão devem ser gerados com entropia suficiente, ter prazo de validade definido e poder ser revogados a qualquer momento pelo servidor, independentemente da expiração.
4. Dados sensíveis (email, senha) não devem ser expostos em logs do sistema.
5. O sistema deve limitar tentativas de login a 5 por minuto por endereço IP para mitigar ataques de força bruta.

### RNF-04 — Compatibilidade e Responsividade

- **Categoria:** Compatibilidade · **Prioridade:** Essencial

**Descrição:** O sistema deve oferecer compatibilidade com navegadores modernos e dispositivos móveis, mantendo responsividade.

**Métrica:** O sistema deve funcionar corretamente nas duas versões mais recentes dos navegadores Google Chrome, Mozilla Firefox, Safari e Microsoft Edge, além de dispositivos com tela a partir de 320 px de largura.

**Restrições:**

1. O layout deve ser responsivo, adaptando-se automaticamente a resoluções de 320 px até 2560 px de largura.
2. Funcionalidades de arrastar e soltar (drag-and-drop) devem possuir alternativa por toque em dispositivos móveis.
3. O editor de layout deve ser funcional tanto em dispositivos desktop quanto em tablets.
4. Nenhuma funcionalidade essencial deve depender de plugins ou extensões de navegador.

### RNF-05 — Personalização Visual sem Quebras de Layout

- **Categoria:** Personalização · **Prioridade:** Importante

**Descrição:** O sistema deve permitir que o usuário personalize temas, cores, subcores e fontes sem comprometer o funcionamento das telas.

**Métrica:** Qualquer combinação de tema e tipografia escolhida pelo usuário deve manter todas as funcionalidades do sistema operacionais, sem quebras de layout ou sobreposição de elementos.

**Restrições:**

1. As personalizações visuais devem ser aplicadas exclusivamente via variáveis CSS, sem alterar a estrutura HTML ou a lógica do sistema.
2. O sistema deve validar combinações de cores antes de aplicá-las, alertando sobre problemas de contraste.
3. Personalizações devem ser aplicadas em tempo real, sem necessidade de recarregar a página.
4. As configurações devem ser persistidas no perfil do usuário e sincronizadas entre dispositivos.

### RNF-06 — Desempenho na Movimentação de Itens

- **Categoria:** Desempenho · **Prioridade:** Importante

**Descrição:** A movimentação de itens entre coleções deve ocorrer com no máximo três comandos e sem atraso perceptível.

**Métrica:** A operação de mover um item deve ser concluída em no máximo 500 milissegundos desde a ação do usuário até a atualização visual na interface.

**Restrições:**

1. A interface deve aplicar a movimentação de forma otimista (atualizar visualmente antes da confirmação do servidor).
2. Em caso de falha na comunicação com o servidor, o sistema deve reverter a movimentação e informar o usuário.
3. A operação de desfazer (Ctrl+Z) deve estar disponível por até 30 segundos após a movimentação, conforme RF-10.
4. As contagens de itens nas coleções de origem e destino devem ser recalculadas imediatamente.

### RNF-07 — Desempenho do Editor de Layout

- **Categoria:** Desempenho · **Prioridade:** Essencial

**Descrição:** O editor de layout deve refletir alterações de elementos em tempo real, sem travamentos ou recarregamentos completos da página.

**Métrica:** Qualquer operação no editor (adicionar, remover, reposicionar ou redimensionar elemento) deve ser refletida na prévia em no máximo 200 milissegundos.

**Restrições:**

1. O editor deve utilizar renderização parcial, atualizando apenas os componentes afetados pela alteração.
2. O salvamento do layout deve ocorrer de forma assíncrona, sem bloquear a interação do usuário com o editor.
3. A prévia deve manter fidelidade visual com o resultado final exibido na ficha do item.
4. O editor deve suportar pelo menos 15 elementos simultâneos sem degradação perceptível de desempenho.

### RNF-08 — Disponibilidade do Sistema

- **Categoria:** Disponibilidade · **Prioridade:** Importante

**Descrição:** O sistema deve manter alta disponibilidade para que atualizações de amigos e sincronização de dados ocorram continuamente.

**Métrica:** O sistema deve garantir disponibilidade mínima de 99% do tempo (uptime), excluindo janelas de manutenção programada comunicadas com antecedência mínima de 24 horas.

**Restrições:**

1. O sistema deve possuir mecanismo de recuperação automática em caso de falha de serviço.
2. Manutenções programadas devem ser realizadas em horários de menor uso (madrugada) e comunicadas com antecedência.
3. O feed de atualizações deve funcionar com conexão intermitente, exibindo dados em cache quando offline.
4. Dados não sincronizados devem ser enviados automaticamente quando a conexão for restabelecida.

### RNF-09 — Acessibilidade

- **Categoria:** Acessibilidade · **Prioridade:** Importante

**Descrição:** O sistema deve adotar padrões de acessibilidade, como contraste adequado, textos legíveis e navegação clara.

**Métrica:** O sistema deve atender aos critérios de conformidade WCAG 2.1 nível AA, incluindo relação de contraste mínima de 4,5:1 para texto normal e 3:1 para texto grande.

**Restrições:**

1. Todos os elementos interativos devem ser acessíveis via navegação por teclado.
2. Imagens informativas devem possuir texto alternativo (alt text) descritivo.
3. O sistema deve alertar o usuário ao selecionar combinações de cores que não atendam ao contraste mínimo.
4. Fontes devem possuir tamanho mínimo de 12 px e ser escaláveis sem perda de funcionalidade até 200% de zoom.

### RNF-10 — Segurança e Integridade dos Dados

- **Categoria:** Segurança · **Prioridade:** Essencial

**Descrição:** O sistema deve armazenar dados em servidor seguro e garantir integridade das coleções criadas pelo usuário.

**Métrica:** O sistema deve realizar backups automáticos diários do banco de dados, com retenção mínima de 30 dias, e garantir que nenhum dado de coleção seja corrompido ou perdido sem ação explícita do usuário.

**Restrições:**

1. O banco de dados deve ser hospedado em servidor com certificação de segurança e acesso restrito.
2. Backups automáticos devem ser realizados diariamente e armazenados em localização separada do servidor principal.
3. Operações de exclusão devem ser registradas em log de auditoria com identificação do usuário, data e hora.
4. O sistema deve utilizar transações atômicas para garantir que operações de escrita não resultem em dados inconsistentes.
5. Arquivos de mídia (imagens, GIFs) devem ser armazenados com redundância para evitar perda.

## Casos de Uso (fluxos)

### Caso de uso: Fazer Login

**Descrição:** Este caso de uso permite que o usuário já cadastrado se autentique no sistema informando seu e-mail e senha, obtendo acesso às funcionalidades restritas da plataforma por meio de um token de sessão gerado automaticamente (RF-02).

**Condições prévias:** O usuário deve estar cadastrado no sistema (RF-01). O usuário não deve possuir sessão ativa.

**Fluxo básico:**

1. O usuário acessa a tela de login do sistema.
2. O usuário insere seu e-mail e senha.
3. O usuário confirma clicando em "Entrar".
4. O sistema valida o formato do e-mail inserido [include: Validar Formato de E-mail].
5. O sistema verifica se o e-mail está cadastrado na base de dados.
6. O sistema compara a senha informada com o hash armazenado.
7. O sistema gera um token de sessão [include: Gerar Token de Sessão].
8. O sistema registra a sessão ativa do usuário.
9. O sistema redireciona o usuário para o dashboard de categorias.

**Fluxos alternativos / exceções:**

- Nos passos 5 ou 6 do FB01, o sistema identifica que o e-mail não está cadastrado ou a senha não corresponde ao hash armazenado.
- O sistema exibe a mensagem genérica: "Credenciais de login inválidas", sem especificar qual campo está incorreto.
- O sistema retorna ao passo 2 do FB01.
- No passo 3 do FB01, o sistema identifica que um ou ambos os campos estão vazios.
- O sistema exibe mensagem de erro indicando os campos obrigatórios.
- O sistema retorna ao passo 2 do FB01.
- O token de sessão atinge o fim de seu prazo de validade, ou a sessão correspondente é revogada pelo sistema (por exemplo, após um logout ou uma alteração de senha).
- Na próxima ação do usuário, o sistema verifica a sessão vinculada ao token e a identifica como inválida.
- O sistema exibe mensagem: "Sua sessão expirou. Por favor, faça login novamente."
- O sistema redireciona o usuário para a tela de login.

**Pós-condições:** O usuário está autenticado e possui sessão ativa. Um token de sessão válido é gerado e armazenado. O usuário tem acesso às funcionalidades restritas da plataforma.

**Requisitos especiais:** RNF02 - A autenticação deve ser concluída em até 3 segundos. RNF03 - A senha nunca deve ser trafegada ou armazenada em texto plano. RNF03 - O token de sessão deve ter prazo de validade definido e poder ser revogado pelo servidor a qualquer momento.

### Caso de uso: Realizar Logout

**Descrição:** Este caso de uso permite que o usuário autenticado encerre sua sessão ativa na plataforma, invalidando o token de sessão e impedindo acesso não autorizado posterior (RF-02).

**Condições prévias:** O usuário deve estar autenticado com sessão ativa.

**Fluxo básico:**

1. O usuário clica na opção "Sair" disponível no menu do sistema.
2. O sistema invalida imediatamente o token de sessão ativo [include: Invalidar Token de Sessão].
3. O sistema encerra a sessão do usuário no servidor.
4. O sistema redireciona o usuário para a tela de login.
5. O sistema exibe mensagem informativa: "Você saiu com sucesso."

**Fluxos alternativos / exceções:**

- No passo 3 do FB01, ocorre falha na comunicação com o servidor.
- O sistema exibe mensagem de erro: "Não foi possível encerrar sua sessão. Tente novamente."
- O sistema mantém o usuário na tela atual.

**Pós-condições:** A sessão do usuário é encerrada. O token de sessão é invalidado e não pode ser reutilizado. O usuário é redirecionado para a tela de login.

**Requisitos especiais:** RNF02 - O logout deve ser concluído em até 2 segundos. RNF03 - O token invalidado não deve ser reutilizável em nenhuma hipótese.

### Caso de uso: Criar Categoria

**Descrição:** Este caso de uso permite que o usuário crie uma nova categoria (agrupador temático de alto nível) para organizar coleções e itens relacionados a um tema, como livros, filmes, séries ou jogos. A categoria é criada vazia e servirá como container para coleções que serão criadas posteriormente. A categoria não possui nível de privacidade próprio; a privacidade é configurada individualmente em cada coleção (RF-06 e RF-08).

**Condições prévias:** O usuário deve estar autenticado no sistema.

**Fluxo básico:**

1. O usuário acessa a funcionalidade de gerenciamento de categorias no dashboard.
2. O usuário seleciona a opção "Criar Categoria".
3. O sistema exibe o formulário de criação de categoria.
4. O usuário insere o nome da categoria.
5. O usuário seleciona um ícone para representar a categoria (opcional).
6. O usuário insere uma descrição para a categoria (opcional).
7. O usuário informa o tipo/tema (opcional), podendo escolher uma das sugestões oferecidas ou digitar o seu próprio.
8. O usuário confirma a criação da categoria.
9. O sistema valida os dados inseridos.
10. O sistema cria a categoria vazia no banco de dados.
11. O sistema exibe mensagem de sucesso.
12. O sistema exibe a categoria recém-criada com atalho destacado para "Criar primeira coleção".

**Fluxos alternativos / exceções:**

- No passo 9 do FB01, o sistema identifica que já existe uma categoria com o mesmo nome para o usuário.
- O sistema exibe mensagem de erro: "Já existe uma categoria com este nome. Por favor, escolha outro nome."
- O sistema retorna ao passo 4 do FB01.
- No passo 9 do FB01, o sistema identifica que o campo nome não foi preenchido.
- O sistema exibe mensagem de erro: "O nome da categoria é obrigatório."
- O sistema retorna ao passo 4 do FB01.
- Em qualquer momento entre os passos 4 e 8 do FB01, o usuário pode selecionar "Cancelar".
- O sistema descarta as informações inseridas.
- O sistema retorna ao dashboard de categorias.
- O caso de uso é encerrado.

**Pós-condições:** Uma nova categoria é criada e armazenada no banco de dados. A categoria aparece no dashboard do usuário. A categoria está pronta para receber coleções, que terão sua própria privacidade individual (padrão pública, conforme RF-08). Nenhum item pode ser cadastrado diretamente na categoria; é necessário criar ao menos uma coleção antes.

**Requisitos especiais:** RNF01 - A interface deve ser intuitiva. RNF02 - O sistema deve criar a categoria em até 3 segundos. RNF04 - Compatibilidade com navegadores modernos e dispositivos móveis.

### Caso de uso: Criar Coleção

**Descrição:** Este caso de uso permite que o usuário crie uma nova coleção dentro de uma categoria existente. A coleção é onde os itens serão efetivamente cadastrados e é também a unidade onde a privacidade é configurada (pública, somente amigos ou privada) e onde o layout é associado (RF-07, RF-15 e RF-08).

**Condições prévias:** O usuário deve estar autenticado. A categoria pai deve existir e pertencer ao usuário.

**Fluxo básico:**

1. O usuário acessa uma categoria específica no dashboard.
2. O usuário seleciona a opção "Criar Coleção".
3. O sistema exibe o formulário de criação de coleção, com privacidade preenchida como "pública" por padrão.
4. O usuário insere o nome da coleção.
5. O usuário seleciona um ícone (opcional).
6. O usuário insere uma descrição (opcional).
7. O usuário associa um layout à coleção (opcional) [E01].
8. O usuário configura a privacidade da coleção (pública, somente amigos ou privada) [E02].
9. O usuário confirma a criação da coleção.
10. O sistema valida os dados inseridos.
11. O sistema cria a coleção vinculada à categoria pai, com a privacidade configurada.
12. O sistema exibe mensagem de sucesso.
13. O sistema redireciona para a visualização da coleção, pronta para receber itens.

**Fluxos alternativos / exceções:**

- No passo 10 do FB01, o sistema identifica que já existe uma coleção com o mesmo nome na categoria pai.
- O sistema exibe mensagem de erro: "Já existe uma coleção com este nome nesta categoria. Por favor, escolha outro nome."
- O sistema retorna ao passo 4 do FB01.
- No passo 10 do FB01, o sistema identifica que o campo nome não foi preenchido.
- O sistema exibe mensagem de erro: "O nome da coleção é obrigatório."
- O sistema retorna ao passo 4 do FB01.
- Em qualquer momento entre os passos 4 e 8 do FB01, o usuário pode selecionar "Cancelar".
- O sistema descarta as informações inseridas.
- O sistema retorna à categoria pai.
- O caso de uso é encerrado.

**Pós-condições:** Uma nova coleção é criada vinculada à categoria pai, com sua privacidade individual configurada. A coleção está pronta para receber itens. Caso um layout tenha sido associado, os itens dessa coleção passam a adotá-lo; caso contrário, utilizam um formulário básico. A contagem de coleções da categoria é atualizada. Se a privacidade configurada for "pública" ou "somente amigos", a coleção poderá ser seguida por amigos (RF-21).

**Requisitos especiais:** RNF01 - A interface deve ser intuitiva. RNF02 - A coleção deve ser criada em até 3 segundos.

### Caso de uso: Criar Item

**Descrição:** Este caso de uso permite que o usuário cadastre um novo item dentro de uma coleção específica. O item adota automaticamente o layout associado à coleção; caso a coleção não tenha layout, utiliza um formulário básico (RF-09 e RF-15).

**Condições prévias:** O usuário deve estar autenticado. A coleção de destino deve existir dentro de uma categoria válida.

**Fluxo básico:**

1. O usuário acessa uma coleção específica (dentro de uma categoria).
2. O usuário seleciona a opção "Adicionar Item".
3. O sistema determina o layout aplicável ao item: o layout associado à coleção, se houver; caso contrário, um formulário básico com campos padrão.
4. O sistema exibe o formulário de criação de item baseado no layout aplicável [E01].
5. O usuário insere o nome do item.
6. O usuário preenche os elementos disponíveis no layout (autor, ano, gênero, avaliação, etc.).
7. O usuário associa uma imagem ao item (opcional) [E02].
8. O usuário confirma a criação do item.
9. O sistema valida os dados inseridos.
10. O sistema cria o item e o vincula à coleção.
11. O sistema exibe mensagem de sucesso.
12. O sistema exibe a ficha completa do item criado.

**Fluxos alternativos / exceções:**

- No passo 3 do FB01, o sistema identifica que a coleção não possui layout associado.
- O sistema exibe formulário básico com campos padrão (nome, descrição, imagem).
- O fluxo continua no passo 5 do FB01.
- No passo 9 do FB01, o sistema identifica que já existe um item com o mesmo nome na coleção.
- O sistema exibe mensagem de alerta: "Já existe um item com este nome nesta coleção. Deseja criar mesmo assim?"
- Se o usuário confirmar, o fluxo continua no passo 10 do FB01.
- Se o usuário cancelar, o sistema retorna ao passo 5 do FB01.
- No passo 9 do FB01, o sistema identifica que o campo nome não foi preenchido.
- O sistema exibe mensagem de erro indicando os campos que precisam ser preenchidos.
- O sistema retorna ao passo 5 do FB01.
- Em qualquer momento entre os passos 5 e 8 do FB01, o usuário pode selecionar "Cancelar".
- O sistema descarta as informações inseridas.
- O sistema retorna à visualização da coleção.
- O caso de uso é encerrado.

**Pós-condições:** Um novo item é criado e vinculado à coleção. O item herda o layout aplicável. A ficha individual do item é gerada automaticamente (RF-11). O item aparece na visualização da coleção. A contagem de itens da coleção e da categoria pai é atualizada.

**Requisitos especiais:** RNF01 - A interface deve ser intuitiva. RNF02 - O sistema deve criar o item em até 3 segundos. RNF07 - Alterações devem refletir em tempo real.

### Caso de uso: Criar Layout Base

**Descrição:** Este caso de uso permite que o usuário crie um layout personalizado através de um editor de elementos, definindo quais campos e elementos visuais farão parte da estrutura de fichas de itens. O layout pode ser criado do zero ou por duplicação de um layout já existente e, posteriormente, associado a uma ou mais coleções, conforme RF-14 e RF-15.

**Condições prévias:** O usuário deve estar autenticado. Para duplicação (FA03), deve existir ao menos um layout prévio de propriedade do usuário.

**Fluxo básico:**

1. O usuário acessa a área de gerenciamento de layouts.
2. O usuário seleciona a opção "Criar Layout Base".
3. O sistema exibe o editor de layout com uma área de trabalho vazia.
4. O usuário insere um nome para o layout.
5. O usuário insere uma descrição para o layout (opcional).
6. O usuário adiciona elementos à área de trabalho (texto, imagem, avaliação, nota, campo personalizado, etc.) [E01].
7. O usuário configura as propriedades de cada elemento (nome do campo, tipo de dado, obrigatoriedade, etc.).
8. O usuário organiza visualmente os elementos na área de trabalho (posicionamento, tamanho, ordem).
9. O sistema exibe prévia do layout em tempo real conforme as modificações.
10. O usuário confirma a criação do layout.
11. O sistema valida a estrutura do layout.
12. O sistema salva o layout no banco de dados.
13. O sistema exibe mensagem de sucesso.
14. O sistema retorna à lista de layouts disponíveis, exibindo o novo layout criado.

**Fluxos alternativos / exceções:**

- No passo 11 do FB01, o sistema identifica que já existe um layout com o mesmo nome.
- O sistema exibe mensagem de erro: "Já existe um layout com este nome. Por favor, escolha outro nome."
- O sistema retorna ao passo 4 do FB01.
- No passo 11 do FB01, o sistema identifica que nenhum elemento foi adicionado ao layout.
- O sistema exibe mensagem de erro: "O layout deve conter pelo menos um elemento."
- O sistema retorna ao passo 6 do FB01.
- No passo 2 do FB01, o usuário pode optar por "Duplicar Layout Existente" [E02].
- O sistema exibe a lista de layouts disponíveis do usuário.
- O usuário seleciona um layout para duplicar.
- O sistema carrega a estrutura do layout selecionado no editor, pré-preenchendo o nome com o original acrescido de "(cópia)".
- O fluxo continua no passo 4 do FB01, com o usuário podendo renomear e ajustar a cópia livremente.
- Alterações realizadas no layout duplicado não refletem no layout de origem.
- Em qualquer momento entre os passos 4 e 10 do FB01, o usuário pode selecionar "Cancelar".
- O sistema exibe mensagem de confirmação: "Deseja descartar as alterações?"
- Se o usuário confirmar, o sistema descarta as informações e retorna à lista de layouts.
- Se o usuário cancelar, o sistema retorna ao editor mantendo as informações.

**Pós-condições:** Um novo layout é criado e armazenado no banco de dados. O layout está disponível para associação a uma ou mais coleções, conforme RF-15. O layout aparece na lista de layouts do usuário. Se criado por duplicação, o layout de origem permanece inalterado.

**Requisitos especiais:** RNF01 - O editor deve ser intuitivo e facilitar a personalização. RNF07 - As alterações devem refletir em tempo real na prévia, sem travamentos. RNF04 - Compatibilidade com navegadores modernos.

### Caso de uso: Visualizar Dashboard de Categorias

**Descrição:** Este caso de uso permite que o usuário visualize todas as suas categorias em uma tela dedicada, com informações resumidas de cada uma (nome, ícone, número de coleções e número total de itens) e acesso rápido às coleções que pertencem a cada categoria. O dashboard é o ponto central de navegação do sistema e permite alternar entre modos de visualização, aplicar filtros e ordenações (RF-13).

**Condições prévias:** O usuário deve estar autenticado no sistema.

**Fluxo básico:**

1. O usuário acessa o menu principal do sistema.
2. O usuário seleciona a opção "Minhas Categorias" ou equivalente.
3. O sistema recupera todas as categorias do usuário no banco de dados.
4. O sistema exibe o dashboard com cards de todas as categorias, aplicando as preferências de visualização previamente salvas (modo, ordenação e filtros).
5. Para cada categoria, o sistema exibe: nome, ícone, contagem de coleções e contagem total de itens (somando todas as coleções).
6. O usuário pode alternar entre modo grade e modo lista [E01].
7. O usuário pode aplicar filtros e ordenações (por nome, data de criação ou tipo/tema da categoria) [E02].
8. O usuário clica em uma categoria para acessar suas coleções.

**Fluxos alternativos / exceções:**

- No passo 3 do FB01, o sistema identifica que o usuário não possui categorias criadas.
- O sistema exibe mensagem informativa: "Você ainda não criou nenhuma categoria. Crie sua primeira categoria para começar!"
- O sistema exibe botão destacado "Criar Nova Categoria".
- Se o usuário clicar no botão, o sistema aciona o caso de uso "Criar Categoria".
- No passo 3 do FB01, o sistema encontra erro ao recuperar as categorias do banco de dados.
- O sistema exibe mensagem de erro: "Não foi possível carregar suas categorias. Por favor, tente novamente."
- O sistema oferece opção para recarregar a página.
- No passo 8 do FB01, o usuário clica em uma categoria que não possui coleções criadas.
- O sistema exibe a página da categoria com mensagem: "Esta categoria ainda não tem coleções. Crie sua primeira coleção para começar a adicionar itens."
- O sistema exibe atalho destacado "Criar Primeira Coleção".
- Se o usuário clicar no atalho, o sistema aciona o caso de uso "Criar Coleção".

**Pós-condições:** O usuário visualiza todas as suas categorias. O usuário pode navegar para qualquer categoria e, dentro dela, para qualquer coleção ou item. O estado de visualização (grade/lista, filtros, ordenação) é persistido para futuras sessões.

**Requisitos especiais:** RNF02 - O sistema deve carregar as categorias em até 3 segundos. RNF04 - O dashboard deve ser responsivo em dispositivos móveis. RNF09 - A interface deve seguir padrões de acessibilidade.

### Caso de uso: Editar Perfil

**Descrição:** Este caso de uso permite que o usuário visualize e edite as informações de seu perfil, incluindo dados cadastrais básicos (nome, e-mail) e personalizações visuais (foto de perfil, descrição, biografia). Alterações no e-mail exigem verificação por meio de link enviado ao novo endereço (RF-05).

**Condições prévias:** O usuário deve estar autenticado no sistema.

**Fluxo básico:**

1. O usuário acessa o menu de configurações ou clica em seu avatar.
2. O usuário seleciona a opção "Editar Perfil".
3. O sistema exibe o formulário de edição com as informações atuais do usuário.
4. O usuário modifica os campos desejados (nome de exibição, biografia, foto) [E01].
5. O usuário pode alterar seu e-mail; o sistema envia link de verificação ao novo endereço e mantém o e-mail antigo ativo até a confirmação [include: Gerenciar Notificações do Sistema].
6. O usuário pode alterar sua senha [E02].
7. O usuário confirma as alterações clicando em "Salvar".
8. O sistema valida os dados inseridos.
9. O sistema atualiza as informações no banco de dados.
10. O sistema exibe mensagem de sucesso: "Perfil atualizado com sucesso."
11. O sistema retorna à visualização do perfil com as informações atualizadas.

**Fluxos alternativos / exceções:**

- No passo 8 do FB01, o sistema identifica que o novo e-mail já está cadastrado por outro usuário.
- O sistema exibe mensagem de erro: "Este e-mail já está em uso. Por favor, utilize outro e-mail."
- O sistema retorna ao passo 5 do FB01.
- No passo 8 do FB01, o sistema identifica que o e-mail inserido possui formato inválido.
- O sistema exibe mensagem de erro: "Por favor, insira um e-mail válido."
- O sistema retorna ao passo 5 do FB01.
- Durante o passo 4 do FB01, o usuário tenta fazer upload de imagem com formato não suportado.
- O sistema exibe mensagem de erro: "Formato de imagem não suportado. Use PNG, JPG ou JPEG."
- O sistema retorna ao passo 4 do FB01.
- Durante o passo 4 do FB01, o usuário tenta fazer upload de imagem maior que 5MB.
- O sistema exibe mensagem de erro: "A imagem deve ter no máximo 5MB."
- O sistema retorna ao passo 4 do FB01.
- O usuário tenta confirmar a alteração de e-mail após o prazo de 24 horas.
- O sistema identifica que o token de verificação expirou.
- O sistema exibe mensagem: "O link de verificação expirou. Por favor, solicite um novo."
- O e-mail original é mantido como endereço ativo.
- Em qualquer momento entre os passos 4 e 7 do FB01, o usuário pode selecionar "Cancelar".
- O sistema exibe mensagem de confirmação se houver alterações não salvas.
- Se o usuário confirmar, o sistema descarta as alterações e retorna à visualização do perfil.
- Se o usuário cancelar a confirmação, retorna ao formulário de edição.

**Pós-condições:** As informações do perfil são atualizadas no banco de dados. As alterações são refletidas em todas as áreas do sistema que exibem informações do usuário. Se o e-mail foi alterado, um e-mail de verificação é enviado e o e-mail antigo permanece ativo até a confirmação. O código de usuário (RF-01) permanece inalterado.

**Requisitos especiais:** RNF01 - A interface deve ser intuitiva. RNF02 - As alterações devem ser salvas em até 3 segundos. RNF03 - O sistema deve proteger informações pessoais. RNF04 - Compatibilidade com dispositivos móveis.

### Caso de uso: Personalizar Interface

**Descrição:** Este caso de uso permite que o usuário configure a aparência visual global da plataforma, incluindo cores, subcores, tipografia e tema do sistema (claro, escuro ou automático). As alterações são exibidas em prévia antes de serem confirmadas e persistem entre sessões e dispositivos (RF-18 e RF-19).

**Condições prévias:** O usuário deve estar autenticado no sistema.

**Fluxo básico:**

1. O usuário acessa a área de configurações da plataforma.
2. O usuário seleciona a opção "Personalizar Interface".
3. O sistema exibe o painel de personalização com as configurações atuais e uma área de prévia.
4. O usuário seleciona ou modifica as configurações desejadas (cores, tipografia, tema) [E01][E02][E03].
5. O sistema atualiza a área de prévia em tempo real conforme as alterações.
6. O usuário confirma as alterações clicando em "Aplicar".
7. O sistema salva as configurações no perfil do usuário.
8. O sistema aplica as alterações globalmente a todas as telas do sistema de forma imediata.
9. O sistema exibe mensagem de sucesso: "Configurações visuais aplicadas com sucesso."

**Fluxos alternativos / exceções:**

- No passo 4 do FB01, o usuário seleciona uma paleta predefinida disponível no sistema (ex.: "Outono", "Oceano").
- O sistema preenche automaticamente os campos de cor primária e subcores com os valores da paleta selecionada.
- O sistema atualiza a prévia em tempo real.
- O fluxo continua no passo 6 do FB01.
- Em qualquer momento entre os passos 4 e 6 do FB01, o usuário seleciona "Cancelar".
- O sistema descarta as alterações não salvas.
- O sistema mantém as configurações visuais anteriores.
- O sistema retorna ao painel de configurações.
- No passo 4 do FB01, o usuário seleciona "Restaurar Configuração Padrão".
- O sistema exibe mensagem de confirmação: "Deseja restaurar todas as configurações visuais para o padrão?"
- Se o usuário confirmar, o sistema restaura os valores padrão e atualiza a prévia.
- O fluxo continua no passo 6 do FB01.

**Pós-condições:** As configurações visuais são salvas no perfil do usuário. As alterações são aplicadas globalmente e imediatamente a todas as telas. As configurações persistem em futuras sessões e em outros dispositivos.

**Requisitos especiais:** RNF01 - O painel de personalização deve ser intuitivo e acessível. RNF07 - As alterações devem ser refletidas na prévia em tempo real, sem recarregar a página. RNF04 - As configurações devem ser persistidas entre sessões e dispositivos.

### Caso de uso: Adicionar Amigo

**Descrição:** Este caso de uso permite que o usuário envie solicitações de amizade para outros usuários da plataforma. A busca pode ser realizada pelo código de usuário ou pelo nome, e a solicitação fica pendente até que o destinatário a aceite ou recuse (RF-20 e RF-24).

**Condições prévias:** O usuário deve estar autenticado no sistema. O usuário destinatário deve existir e ter um perfil ativo.

**Fluxo básico:**

1. O usuário acessa a área de busca de usuários ou área social.
2. O usuário insere o código do usuário ou o nome da pessoa que deseja adicionar.
3. O sistema busca usuários correspondentes no banco de dados, conforme o critério informado.
4. O sistema exibe uma lista de resultados com nome, foto e código de usuário.
5. O usuário identifica a pessoa desejada e clica em "Adicionar Amigo".
6. O sistema verifica se já existe uma relação de amizade ou solicitação pendente.
7. O sistema cria uma solicitação de amizade pendente no banco de dados.
8. O sistema gera notificação in-app ao destinatário informando sobre a solicitação [include: Gerenciar Notificações do Sistema].
9. O sistema exibe mensagem de sucesso: "Solicitação de amizade enviada."
10. O sistema atualiza o botão para "Solicitação Pendente" no perfil do usuário destinatário.

**Fluxos alternativos / exceções:**

- No passo 3 do FB01, o sistema não encontra nenhum usuário correspondente ao critério informado.
- O sistema exibe mensagem: "Nenhum usuário encontrado."
- O sistema retorna ao passo 2 do FB01.
- No passo 6 do FB01, o sistema identifica que os usuários já são amigos.
- O sistema exibe mensagem: "Vocês já são amigos."
- O sistema oferece opção de visualizar o perfil do amigo.
- O caso de uso é encerrado.
- No passo 6 do FB01, o sistema identifica que já existe uma solicitação pendente.
- O sistema exibe mensagem: "Você já enviou uma solicitação de amizade para este usuário."
- O sistema oferece opção de cancelar a solicitação pendente.
- O caso de uso é encerrado.
- No passo 6 do FB01, o sistema identifica que existe uma solicitação pendente enviada pelo usuário que está sendo adicionado.
- O sistema exibe mensagem: "Este usuário já enviou uma solicitação de amizade para você."
- O sistema oferece opção de aceitar a solicitação existente.
- Se o usuário aceitar, o sistema aciona o caso de uso "Gerenciar Solicitações de Amizade".

**Pós-condições:** Uma solicitação de amizade é criada com status "pendente". O destinatário recebe notificação da solicitação (RF-24). A solicitação aparece na lista de pendentes de ambos os usuários. O botão no perfil do destinatário muda para "Solicitação Pendente".

**Requisitos especiais:** RNF02 - A busca e envio de solicitação devem ocorrer em até 3 segundos. RNF08 - O sistema deve enviar notificações em tempo real. RNF03 - A privacidade dos usuários deve ser respeitada.

### Caso de uso: Visualizar Feed de Atualizações

**Descrição:** Este caso de uso permite que o usuário acompanhe em tempo real as atualizações das coleções que segue, visualizando eventos de adição, edição e remoção de itens realizados por seus proprietários, ordenados cronologicamente do mais recente ao mais antigo (RF-22).

**Condições prévias:** O usuário deve estar autenticado. O usuário deve seguir ao menos uma coleção com privacidade pública ou somente amigos (RF-21).

**Fluxo básico:**

1. O usuário acessa a área de feed de atualizações.
2. O sistema identifica todas as coleções que o usuário segue ativamente (conforme RF-21).
3. O sistema recupera os eventos de atualização associados a essas coleções (adição/edição/remoção de itens), ordenados do mais recente ao mais antigo.
4. O sistema verifica a privacidade atual de cada coleção antes de exibir seus eventos (RF-08).
5. O sistema exibe o feed com os eventos, contendo: nome do autor da ação, tipo de evento, nome do item afetado, nome da coleção, nome da categoria pai e data/hora relativa do evento.
6. O feed é atualizado automaticamente em tempo real sem necessidade de recarregar a página.
7. O usuário pode rolar o feed para carregar eventos mais antigos progressivamente (paginação).

**Fluxos alternativos / exceções:**

- No passo 2 do FB01, o sistema identifica que o usuário não segue nenhuma coleção.
- O sistema exibe mensagem: "Seu feed está vazio. Siga coleções para acompanhar suas atualizações."
- O sistema exibe atalho para a área de descoberta de perfis e coleções públicas.
- No passo 4 do FB01, o sistema identifica que uma coleção anteriormente seguida foi alterada para privada.
- O sistema não exibe os eventos dessa coleção, incluindo eventos anteriores à mudança de privacidade.
- O sistema encerra automaticamente o acompanhamento dessa coleção.
- Os demais eventos do feed continuam sendo exibidos normalmente.
- No passo 3 do FB01, o sistema encontra erro ao recuperar os eventos.
- O sistema exibe mensagem: "Não foi possível carregar o feed. Tente novamente."
- O sistema oferece botão para recarregar.

**Pós-condições:** O usuário visualiza os eventos recentes das coleções que segue. O feed permanece atualizado em tempo real durante a navegação. Coleções tornadas privadas são removidas do feed imediatamente, com seu acompanhamento encerrado.

**Requisitos especiais:** RNF07 - O feed deve ser atualizado em tempo real sem recarregar a página. RNF02 - O carregamento inicial do feed deve ocorrer em até 3 segundos. RNF03 - Eventos de coleções privadas não devem ser exibidos em nenhuma circunstância.

### Caso de uso: Gerenciar Solicitações de Amizade

**Descrição:** Este caso de uso permite que o usuário visualize, aceite ou recuse solicitações de amizade recebidas de outros usuários da plataforma. Ao aceitar, o vínculo de amizade é criado mutuamente e o solicitante é notificado. Ao recusar, a solicitação é marcada como recusada sem notificação ao solicitante (RF-20 e RF-24).

**Condições prévias:** O usuário deve estar autenticado. Deve existir ao menos uma solicitação de amizade com status "pendente" endereçada ao usuário.

**Fluxo básico:**

1. O usuário acessa a área de notificações ou de solicitações pendentes.
2. O sistema exibe a lista de solicitações de amizade com status "pendente", contendo nome, foto e data de envio de cada solicitante.
3. O usuário seleciona a solicitação desejada e clica em "Aceitar".
4. O sistema atualiza o status da solicitação para "aceita" no banco de dados.
5. O sistema cria o vínculo de amizade mútuo entre os dois usuários.
6. O sistema envia notificação ao solicitante informando que a solicitação foi aceita [include: Gerenciar Notificações do Sistema].
7. O sistema exibe mensagem de sucesso: "Vocês agora são amigos!"
8. O sistema atualiza a lista de solicitações, removendo a solicitação aceita.

**Fluxos alternativos / exceções:**

- No passo 3 do FB01, o usuário clica em "Recusar" na solicitação desejada.
- O sistema atualiza o status da solicitação para "recusada" no banco de dados.
- O sistema não gera notificação ao solicitante; a recusa é silenciosa.
- O sistema remove a solicitação da lista de pendentes do usuário.
- O caso de uso é encerrado.
- No passo 2 do FB01, o sistema identifica que não há solicitações pendentes.
- O sistema exibe mensagem: "Você não possui solicitações de amizade pendentes."
- O caso de uso é encerrado.
- No passo 3 do FB01, o usuário tenta aceitar uma solicitação que foi cancelada pelo remetente antes da resposta.
- O sistema exibe mensagem: "Esta solicitação não está mais disponível."
- O sistema remove a solicitação da lista de pendentes.
- O caso de uso é encerrado.

**Pós-condições:** Se aceita: vínculo de amizade mútuo criado; ambos os usuários aparecem na lista de amigos um do outro e o solicitante é notificado. Se recusada: status da solicitação atualizado para "recusada"; nenhum vínculo ou notificação é criado.

**Requisitos especiais:** RNF08 - Notificações de aceite devem ser enviadas em tempo real; recusas não geram evento. RNF02 - A operação deve ser concluída em até 3 segundos.

### Caso de uso: Visualizar Perfil de Amigo

**Descrição:** Este caso de uso permite que o usuário acesse o perfil público de outro usuário (amigo ou não), visualizando suas informações básicas e as coleções disponíveis conforme o nível de privacidade configurado em cada uma delas. As coleções visíveis são agrupadas pelas respectivas categorias, e a navegação nas coleções e itens do usuário visitado é restrita ao modo somente leitura (RF-23 e RF-08).

**Condições prévias:** O usuário deve estar autenticado. O usuário visitado deve existir na plataforma.

**Fluxo básico:**

1. O usuário acessa a lista de amigos ou realiza busca pelo código do usuário desejado.
2. O usuário seleciona o perfil desejado.
3. O sistema recupera as informações públicas do perfil (nome, foto, biografia).
4. O sistema recupera as coleções do usuário visitado com privacidade "pública" ou "somente amigos" (esta última apenas quando há vínculo de amizade confirmada), agrupando-as pelas respectivas categorias.
5. O sistema exibe o perfil com suas informações e as coleções acessíveis, agrupadas por categoria.
6. Coleções com privacidade "privada" não são listadas nem mencionadas. Categorias que, para esse visitante, não tenham nenhuma coleção visível também são ocultadas integralmente.
7. O usuário navega pelas coleções visíveis e pelos itens contidos nelas, em modo somente leitura.
8. O usuário pode seguir uma coleção pública independentemente de amizade; para coleções "somente amigos", o acompanhamento exige amizade confirmada [E01].

**Fluxos alternativos / exceções:**

- No passo 4 do FB01, o sistema identifica que todas as coleções do usuário visitado são privadas ou que ele não possui coleções acessíveis ao visitante.
- O sistema exibe o perfil básico do usuário visitado com a mensagem: "Este usuário não possui coleções públicas no momento."
- O caso de uso continua sem exibir coleções ou categorias.
- O usuário tenta acessar uma coleção privada de outro usuário por meio de link direto.
- O sistema verifica a privacidade da coleção.
- O sistema exibe mensagem: "Esta coleção não está disponível."
- O sistema não exibe nenhum dado da coleção, nem mesmo o nome.
- No passo 4 do FB01, o sistema identifica que não existe vínculo de amizade confirmado entre os usuários.
- O sistema exibe apenas as coleções com privacidade "pública" do perfil visitado, agrupadas pelas respectivas categorias.
- Coleções com privacidade "somente amigos" não são exibidas.
- Categorias sem nenhuma coleção pública visível são ocultadas integralmente.

**Pós-condições:** O usuário visualiza o perfil e as coleções acessíveis do usuário visitado, agrupadas por categoria. Nenhuma alteração é feita nos dados do usuário visitado. O usuário pode iniciar o fluxo de seguir uma coleção a partir desta tela.

**Requisitos especiais:** RNF03 - A privacidade das coleções deve ser respeitada rigorosamente em toda a navegação. RNF02 - O perfil deve ser carregado em até 3 segundos. RNF04 - A visualização deve ser responsiva em dispositivos móveis.
