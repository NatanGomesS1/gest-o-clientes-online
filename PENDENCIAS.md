# Plano e pendências do sistema

Ordem combinada: **Cadastro único → Ficha 360° → Serviços → Resultados por competência → Manual**.
Não mexer agora em prioridade, obrigações, Foco Agora, atividades nem Sites e Ferramentas.

## Etapa 1: Cadastro único ✅ (V2.4.0, 22/out/2026)
Empresa nasce uma vez (Prospect, Cliente, Pausado, Encerrado). A oportunidade aponta para a empresa (`possiveis.empresa_id`).
- Ficou para depois: pagamentos, histórico e ações ainda ligam ao cliente pelo **nome**. Trocar para ID junto da Etapa 3.

## Etapa 2: Ficha 360° (próxima)
Abas: Visão geral | Atividades | Pendências | Monitoramento | Métricas | Diagnóstico | Histórico | Relatório.
A Visão geral precisa responder sozinha: como está o cliente e o que fazer agora.

### Itens vindos do gerador de relatórios (22/out/2026, conversa "JC Artefatos September report")
O gerador (https://natangomess1.github.io/gerador-relatorios/, repositório `NatanGomesS1/gerador-relatorios`) não lê nada sozinho: o Natan copia do sistema e cola nele.

1. **Atividades do mês por canal, prontas para copiar** (aba Atividades / Relatório)
   - Na Ficha, um botão "Copiar atividades do mês" que gere o texto separado em 3 blocos: **Perfil no Google**, **Anúncios** e **Site**.
   - Uma ação por linha, em linguagem de cliente. Canal sem atividade fica vazio.
   - Cada bloco é colado num dos 3 campos "O que fizemos no mês" do gerador.
   - Hoje `__ATIVIDADES_MES__` vem vazio, e o gerador não usa mais esse campo.

2. **Aviso de poucas avaliações** (aba Métricas)
   - Guardar o total de avaliações do Perfil no Google e avisar quando for baixo (o gerador usa **menos de 10**).
   - Exemplo: a JC Artefatos tem só 1 avaliação.
   - Mandar na tabela como `__GBP_AVALIACOES__`. O gerador já lê esse campo e cria a página sobre avaliações e o passo "pedir avaliação aos clientes".

3. **Pedidos de rota: padronizar** (aba Métricas / Relatório)
   - O Natan não usa rota como resultado, porque o Google infla esse número. Depende do tipo de cliente.
   - `__GBP_ACOES__` está inconsistente:
     - na TR Reboque veio **sem** rotas (60 = 25 + 33 + 2);
     - na JC veio **com** rotas (46 inclui 31 de rota).
   - `__GBP_SERVICOS_EST__`, `__GBP_FATURAMENTO_EST__`, `__GBP_ROAS__` e `__RESUMO_MES__` também contam as rotas. Na JC, o app diz 13 serviços e R$ 2.600; o gerador, sem rotas, diz 4 serviços e R$ 800.
   - Sugestão: opção por cliente "contar rotas como resultado", **desligada por padrão**, aplicada igual em todos os campos.
   - **Loja física** (ex.: Eletroled, material elétrico): configuração por cliente "Tipo de negócio: loja física".
     - Mostra as **visitas à loja** (Store visits do Ads, estimativa do Google) num bloco separado, que não soma nos contatos.
     - Pedidos de rota continuam fora do resultado.
     - Liga a nota "Sobre os números" no relatório: anúncio direto para o WhatsApp + venda no balcão = o relatório mostra o mínimo garantido, não o total.
     - Até existir a configuração, a decisão fica anotada na Observação da Ficha do cliente.

4. **Nome do contato do cliente** (cadastro / aba Relatório)
   - `__PRIMEIRO_NOME__` hoje traz o nome do responsável da agência ("Natan").
   - A mensagem de WhatsApp do gerador precisa do primeiro nome de quem recebe do lado do cliente ("Oi, Jorge!").
   - Sugestão: campo próprio no cadastro, enviado como `__CONTATO_NOME__`.

### Cliente de referência
- **JC Artefatos:** vende e entrega artefatos de cimento (cobogó etc.). Sem anúncios no Google: só Perfil no Google e acompanhamento (R$ 50/mês).
- **Eletroled (Cabo Frio):** loja de material elétrico, atendimento presencial. Anúncios vão direto para o WhatsApp, sem landing page. Tem a conversão "Store visits" ativa no Ads.

## Integração com os agentes (decisão do Natan, 23/out/2026): registrar, implementar só quando ele pedir
**O app é a fonte da verdade do dia a dia** (clientes, funil, ações, prioridades, resultados e aprendizados). Os agentes do `site-agropet` guardam só o método e a ficha estratégica. Nada de duas memórias.
Especificação completa: repositório `site-agropet`, branch `claude/agente-anuncios` (PR #7), arquivo `integracao-app.md`.

- **Entrada (app → agente):** já existe. É o "Exportar backup" (`exportarJSON`). O agente lê as ações (abertas, e as fechadas com `resultado`/`resultadoTxt`/`aprendizado`), clientes, possiveis e historico.
- **Saída (agente → app):** cada agente entrega um bloco JSON:
  `{"tipo":"ntn_acoes_agente","schemaVersion":2,"agente":"anuncios","geradoEm":"ISO","acoes":[{ id, cliente, servico, tipo, origem, texto, proximaAcao, data, quando, prazo, status:"aberto", statusOperacional, impacto, urgencia, confianca, esforco, estimativaMin, obrigacaoContratual, riscoFinanceiro, evidencia }]}`
  - Campos e valores seguem o `schemaV2JSON()`.
  - `origem` = `"diagnostico"` (o enum não tem "agente").
  - `id` = `ag-<agente>-<cliente>-<AAAAMMDD>-<nn>`.
  - `cliente` = nome exato do cadastro.

**A fazer no app: botão "Colar ações do agente" na Central de Trabalho**
1. Caixa de texto para colar o JSON (sem arquivo).
2. Validar:
   - `tipo` e `schemaVersion`;
   - os valores de cada campo;
   - as faixas: impacto 1–5, urgencia 1–5, confianca 1–3, esforco 1–5, riscoFinanceiro 0–5;
   - se o cliente existe.
3. **Prévia antes de gravar:** o que será criado, atualizado ou ignorado, com a prioridade calculada (`prioridadeAcao`).
4. Gravar com `persist('acoes', …)`, juntando por `id`:
   - id novo → cria;
   - id aberto → atualiza;
   - id concluído ou cancelado → não mexe e avisa na prévia.
5. Cliente não encontrado → não grava aquela ação e mostra o nome para corrigir.
6. **Nunca** apagar nada. **Nunca** sobrescrever `resultado`, `resultadoTxt`, `aprendizado` ou `minutosAcumulados` que já existam.
7. O aprendizado continua como hoje: caixa "Vale testar em outros clientes" ao fechar a ação.

- **Primeiro pacote para testar:** `clientes/sunleg/ficha.md` no `site-agropet` (seção "Primeiras ações para colar no app"), com 4 ações.
- **Confirmar com o Natan:** o nome exato da Sunleg no cadastro do app.
- **Ligação com o plano:** encaixa na Central de Trabalho, sem mexer em prioridade, obrigações nem Foco Agora. Quando a Etapa 3 trocar a ligação por ID, o `cliente` do JSON pode passar a aceitar também o ID da empresa.

## Grade de Posições → app (ideia do Natan, 24/out/2026): registrar, implementar só quando ele pedir
Hoje a tela **Ranking Google** guarda **uma posição por cliente** em cada termo, digitada à mão. A Grade de Posições (`gerador-relatorios/grade.html`) mede **9 ou 25 pontos** por busca, e o resto se perde ao resumir.

**Lado da grade (feito no `gerador-relatorios`):** botão "Copiar para o app" que gera um bloco JSON:
`{"tipo":"ntn_grade","schemaVersion":1,"cliente","busca","data","centro":{lat,lng},"n","km","pontos":[{"lin","col","pos"("x" = não apareceu, null = não medido),"primeiro"(id do concorrente ou null)}],"concorrentes":[{"id","nome","categoria","avaliacoes","nota"}],"resumo":{"top3","total","media","naoApareceu"}}`

**A fazer no app: "Colar grade" na tela Ranking** (pode ser a mesma caixa do "Colar ações do agente", decidindo pelo `tipo`)
1. Valida `tipo`, `schemaVersion` e se o `cliente` existe. Nome não encontrado → pergunta qual cliente antes de gravar.
2. Cria ou atualiza o termo (`busca`) daquele cliente. A posição do card passa a ser a **média**, com a linha "top 3 em X de Y pontos".
3. Guarda a grade inteira no histórico do termo (o `hist` que já existe), para a evolução por cliente.
4. **Só compara medições iguais:** mesmo centro, mesmo tamanho (n) e mesma distância (km). Se mudar, avisa "não comparável com a anterior" em vez de mostrar seta de subiu/caiu.
5. No card: grade pequena colorida (verde top 3, amarelo 4–10, vermelho fora) da medição atual ao lado da anterior.
6. Concorrentes: mostra quem ganha mais pontos e a evolução das avaliações dele mês a mês.
7. Nunca apaga medição anterior.

## Etapa 3: Serviços contratados
Cada serviço com situação própria (GBP ativo, Ads ativo, Site concluído/manutenção), alimentando Obrigações, Central e Relatórios.

## Etapa 4: Resultados por competência
Cliente + mês/ano com métricas de GBP, Ads e Site, atividades, monitoramentos, resumo e próximas ações. Os campos devem seguir os mesmos nomes da tabela do gerador (`__ADS_ACOES__`, `__GBP_ACOES__`, `__GBP_AVALIACOES__`, `__CONTATO_NOME__`…).

## Etapa 5: Manual "Como usar"
Só depois das etapas acima, com imagens reais do sistema.
