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

## Etapa 3: Serviços contratados
Cada serviço com situação própria (GBP ativo, Ads ativo, Site concluído/manutenção), alimentando Obrigações, Central e Relatórios.

## Etapa 4: Resultados por competência
Cliente + mês/ano com métricas de GBP, Ads e Site, atividades, monitoramentos, resumo e próximas ações. Os campos devem seguir os mesmos nomes da tabela do gerador (`__ADS_ACOES__`, `__GBP_ACOES__`, `__GBP_AVALIACOES__`, `__CONTATO_NOME__`…).

## Etapa 5: Manual "Como usar"
Só depois das etapas acima, com imagens reais do sistema.
