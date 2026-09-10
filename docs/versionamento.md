# Versionamento — Jornada e loop | Etapa 0 — Fundação

## Status

**Atividade:** Atividade Real 02 — PromptOps Academy  
**Frente oficial:** Jornada e loop  
**Responsável:** Levi  
**Etapa:** 0 — Fundação  
**Tipo:** Proposta técnica para validação  
**Status:** Em revisão

Este documento organiza a proposta de versionamento da frente de Jornada e loop.

As regras identificadas como proposta não representam decisão oficial da equipe até serem validadas na Etapa 0.

A estrutura de dados utilizada pela implementação deve seguir o schema canônico definido pela equipe em:

`docs/modelo-de-dados.md`

Em caso de divergência entre este documento e o schema canônico aprovado, prevalece o modelo de dados aprovado pela equipe.

---

## 1. Objetivo

Definir um fluxo de versionamento que permita:

- identificar qual versão foi utilizada em cada teste;
- preservar versões e resultados anteriores;
- registrar falhas e ajustes;
- comparar uma versão antes e depois de uma melhoria;
- manter rastreabilidade entre prompt, versão e teste.

O objetivo é apoiar o fluxo de melhoria contínua previsto para o PromptOps Academy sem perder o histórico das execuções.

---

## 2. Base confirmada da Atividade 02

O Guia de Produto e Execução estabelece como parte do fluxo do produto:

`Versão → Teste → Avaliação → Ajuste → Novo teste`

O registro de teste precisa manter informações suficientes para identificar:

- prompt e versão;
- entrada;
- resultado esperado;
- resultado obtido;
- avaliação;
- falha;
- ajuste;
- responsável;
- data;
- próximo teste.

Também é necessário preservar o vínculo entre um teste e a versão utilizada naquela execução.

Esta especificação detalha uma proposta operacional para atender esses requisitos.

---

## 3. Fluxo proposto

```text
CRIAR PROMPT
      ↓
   RASCUNHO
      ↓
pronto para teste
      ↓
      v1
      ↓
    TESTE
      ↓
  AVALIAÇÃO
      ↓
┌──────────────────────────────────┐
│                                  │
resultado adequado        falha/oportunidade
│                                  │
mantém versão              registrar evidência
                                   ↓
                             propor ajuste
                                   ↓
                       alteração relevante?
                           ↙           ↘
                         NÃO           SIM
                          ↓             ↓
                  ajuste editorial   nova versão
                                         ↓
                                      RETESTE
                                         ↓
                                     HISTÓRICO

O fluxo acima é uma proposta da frente de Jornada e loop para validação na Etapa 0.

4. Regras propostas
RV-01 — Primeira versão

Propõe-se que o prompt permaneça como rascunho enquanto ainda estiver sendo preparado.

Quando estiver pronto para o primeiro teste, passa a existir como v1.

RV-02 — Preservação do histórico

Depois que uma versão possuir teste registrado, alterações relevantes não devem sobrescrever silenciosamente o histórico daquela execução.

A versão anterior deve continuar disponível para consulta.

RV-03 — Teste vinculado à versão

Todo teste deve permitir identificar exatamente a versão utilizada na execução.

O relacionamento e os nomes de propriedades devem seguir docs/modelo-de-dados.md.

RV-04 — Evidência da execução

O registro precisa preservar evidência suficiente para impedir que um teste antigo pareça pertencer a um conteúdo diferente daquele realmente executado.

Caso o schema canônico utilize um campo específico para snapshot do texto, deve ser utilizado o nome definido em docs/modelo-de-dados.md.

RV-05 — Avaliação

A avaliação deve utilizar os estados definidos no schema canônico.

No padrão atualmente alinhado pela equipe, o estado intermediário é:

ajustar

A avaliação deve considerar critérios definidos para o teste, e não somente uma percepção subjetiva sobre a resposta.

RV-06 — Gatilho para nova versão

Proposta para validação na Etapa 0:

criar uma nova versão quando uma alteração puder modificar o comportamento, interpretação, formato ou resposta esperada do prompt.

Exemplos:

objetivo;
instrução principal;
contexto relevante;
constraints;
formato esperado;
critérios de qualidade;
próxima ação.

O gatilho definitivo para criação de uma nova versão depende da aprovação da Etapa 0.

RV-07 — Correções editoriais

Proposta para validação:

uma correção puramente editorial que não altere o significado ou comportamento esperado pode permanecer na mesma versão, desde que exista rastreabilidade da alteração conforme o schema canônico.

Exemplo:

"Analize os gargalos." → "Analise os gargalos."

RV-08 — Alteração de impacto incerto

Proposta para validação:

quando não for possível determinar com segurança se uma alteração modifica o comportamento esperado do prompt, priorizar a criação de nova versão para preservar a rastreabilidade.

RV-09 — Versão atual

A identificação da versão atual deve utilizar a relação definida no schema canônico.

Versões anteriores permanecem disponíveis para histórico e comparação.

RV-10 — Justificativa da alteração

A falha e o ajuste registrados no teste devem fornecer a principal evidência para explicar uma evolução de versão.

Informações complementares sobre a mudança devem utilizar apenas os campos existentes no schema canônico.

RV-11 — Reteste

Uma nova versão não deve ser considerada melhoria apenas por ter sido criada.

Ela precisa ser testada.

Quando possível, o reteste deve reutilizar a mesma entrada e os mesmos critérios para facilitar a comparação.

RV-12 — Evidência de melhoria

Uma melhoria deve ser demonstrada por evidência de testes vinculados às respectivas versões.

Exemplo:

v1
└── T01 → reprovado
      ↓
   ajuste
      ↓
v2
└── T02 → aprovado

A criação da v2, isoladamente, não comprova melhoria.

5. Integração com o schema canônico

Este documento não redefine o modelo de dados.

A fonte de verdade para:

entidades;
nomes de campos;
tipos;
valores permitidos;
IDs;
relações;
referências;
datas;
status;

é:

docs/modelo-de-dados.md

A implementação de versionamento deve utilizar os nomes padronizados definidos nesse arquivo.

Entre os alinhamentos informados pela equipe estão:

constraints para restrições;
createdAt para data de criação;
ajustar como estado intermediário de avaliação/status correspondente.

Novos campos não devem ser tratados como obrigatórios apenas por aparecerem nesta proposta.

Se uma necessidade de versionamento exigir alteração do schema canônico, ela deve ser discutida com a frente de Dados e persistência antes da implementação.

6. Integridade dos dados

O versionamento deve respeitar as validações de integridade definidas no modelo de dados.

Em especial:

IDs devem ser válidos e únicos;
um teste só pode referenciar uma versão existente;
uma versão deve pertencer ao prompt correto;
referências não devem apontar para registros inexistentes;
alterações não devem apagar silenciosamente versões ou testes históricos;
JSON recuperado deve ser validado antes de ser utilizado.

Falhas de leitura ou gravação não devem ser apresentadas ao usuário como sucesso.

7. Privacidade e segurança

O PromptOps Academy utiliza JSON e localStorage no MVP.

Esses mecanismos não devem ser tratados como armazenamento seguro para informações sensíveis.

Para testes, demonstrações e dados persistidos localmente, propõe-se:

utilizar dados fictícios ou autorizados;
não armazenar senhas;
não armazenar tokens ou chaves de API;
evitar dados pessoais ou confidenciais desnecessários;
não registrar informações sensíveis dentro de entradas, resultados ou evidências de teste;
validar dados recuperados do JSON/localStorage;
tratar conteúdo de prompts e resultados como texto, evitando renderização arbitrária de HTML;
avisar quando uma operação de persistência falhar;
revisar o conteúdo antes de qualquer exportação;
não incluir dados sensíveis em exportações.

As regras finais de privacidade devem permanecer alinhadas à frente de Governança e aceite e às decisões da equipe.

8. Evidências e QA

A validação do versionamento deve utilizar os cenários de QA definidos pelo projeto.

São diretamente relacionados a esta especificação:

Q09

Selecionar uma versão anterior e copiar.

Esperado: copiar exatamente a versão exibida.

Q10

Registrar uma falha, criar uma nova versão e repetir o teste.

Esperado: cada teste permanecer vinculado à versão correta.

Também devem ser registradas:

entrada;
resultado esperado;
resultado obtido;
avaliação;
responsável;
data;
evidência da execução.

Nenhum teste deve ser marcado como executado antes da execução real.

9. Dependências

Esta especificação depende de:

docs/modelo-de-dados.md — schema canônico;
critérios de aceite da Etapa 0;
validações de integridade;
regras de privacidade e proteção de dados;
revisão cruzada com as frentes relacionadas.

A frente de Jornada e loop não deve alterar unilateralmente o contrato de dados.

10. Status para aprovação

A lógica deste documento permanece como proposta da frente de Jornada e loop.

Antes de ser considerada aprovada:

deve estar alinhada ao schema canônico;
deve passar pela revisão da equipe;
eventuais divergências devem ser resolvidas;
a validação final da Etapa 0 permanece com Rogério.

Nenhuma regra marcada como proposta deve ser tratada como decisão oficial antes dessa validação.
