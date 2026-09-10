# Regras e Fluxo de Versionamento — PromptOps Academy

## Status

Proposta elaborada pela frente de **Jornada e Loop** para validação na Etapa 0.

Responsável pela especificação: **Levi**

A validação final depende da equipe e de Rogério.

---

## 1. Objetivo

Definir como os prompts serão criados, testados, versionados, corrigidos e preservados ao longo do tempo.

O versionamento deve permitir identificar:

- qual versão está atualmente em uso;
- qual versão foi utilizada em cada teste;
- quais alterações foram realizadas;
- quais falhas motivaram uma nova versão;
- se a nova versão realmente apresentou melhoria;
- quais correções foram apenas editoriais.

---

## 2. Criação inicial — Rascunho → v1

Enquanto um prompt ainda estiver sendo preparado e não tiver sido testado, ele permanece como **rascunho** e pode ser editado normalmente.

Quando estiver pronto para o primeiro teste, passa a existir como:

`v1`

Exemplo:

```text
Prompt OP-02
└── v1 ← pronta para o primeiro teste

Depois que a v1 receber um teste, ela passa a fazer parte do histórico e não deve mais ser sobrescrita.

3. Vínculo permanente entre teste e versão

Todo teste deve registrar exatamente qual versão foi realmente executada.

Exemplo:

OP-02
├── v1
│   └── T01
│
└── v2
    └── T02

O teste T01 pertence à v1 e nunca deve posteriormente aparecer como um teste da v2.

No modelo de dados, esse vínculo é representado por:

versionId

Regra

Teste e versão formam um vínculo permanente.

4. Registro de um teste

Cada teste deve registrar:

promptId;
versionId;
textSnapshot;
entrada utilizada;
resultado esperado;
resultado obtido;
avaliação;
falha encontrada;
ajuste recomendado;
responsável;
data;
próximo teste.
textSnapshot

textSnapshot deve preservar uma cópia exata do texto do prompt utilizado no momento da execução.

Isso garante que o histórico continue fiel mesmo que posteriormente seja realizada uma correção editorial na mesma versão.

Exemplo:

T01
versionId: OP-02-v1
textSnapshot: texto exato utilizado naquele teste
5. Avaliação do teste

A avaliação deve utilizar somente três estados:

aprovado
ajustar
reprovado
aprovado

O resultado cumpriu os critérios definidos para o teste.

ajustar

O resultado funcionou parcialmente, mas existe algo que precisa ser melhorado.

reprovado

O resultado falhou em um critério essencial.

Regra

A avaliação deve considerar critérios definidos antes do teste, e não apenas se a resposta pareceu boa.

6. Quando criar uma nova versão

Depois de um teste e de uma possível alteração, deve ser feita a seguinte pergunta:

Essa mudança é capaz de alterar o comportamento, a interpretação, o formato ou a resposta esperada da IA?

Se sim, deve ser criada uma nova versão.

Exemplos:

mudança no objetivo;
alteração da instrução principal;
adição ou remoção de contexto relevante;
alteração de restrições;
mudança no formato esperado;
alteração de critérios de qualidade;
mudança na próxima ação;
qualquer alteração operacional capaz de produzir uma resposta diferente.

Fluxo:

v1
↓
teste
↓
falha ou oportunidade
↓
ajuste recomendado
↓
alteração operacional
↓
v2
Regra simples

Mudança capaz de alterar o comportamento esperado do prompt = nova versão.

7. Ajuste editorial sem nova versão

Uma alteração claramente editorial, que não modifica o significado nem o comportamento esperado do prompt, não deve gerar uma nova versão.

Exemplo:

Antes:
"Analize os gargalos."

Depois:
"Analise os gargalos."

Nesse caso, a versão continua sendo a mesma.

A alteração deve ser registrada em:

editorialChanges

Cada registro deve preservar:

data;
autor;
descrição;
conteúdo anterior;
conteúdo posterior.

Exemplo:

Versão: v2
Tipo: ajuste editorial
Antes: "Analize"
Depois: "Analise"
Autor: Levi
Data: 2026-09-09
Regra

Mudança puramente editorial mantém a versão, mas deve deixar histórico.

8. Caso de dúvida

Se não estiver claro se uma alteração pode ou não modificar o comportamento da IA:

criar uma nova versão.

É preferível criar uma nova versão do que classificar incorretamente uma alteração relevante como simples ajuste editorial.

9. Preservação do histórico

Depois que uma versão tiver sido testada, ela não deve ser apagada nem sobrescrita por uma alteração operacional.

Errado:

v1
↓
editar
↓
substituir a própria v1

Correto:

v1 ← preservada
v2 ← nova versão

As versões anteriores devem continuar disponíveis para consulta.

10. Versão atual

Cada Prompt deve informar qual versão está atualmente ativa.

Isso é representado por:

currentVersionId

Exemplo:

OP-02
├── v1 — histórica
├── v2 — histórica
└── v3 — atual

Apenas uma versão deve ser considerada atual por vez.

As anteriores continuam preservadas.

11. Justificativa de uma nova versão

A principal evidência da necessidade de uma nova versão deve estar no teste que originou a alteração.

O teste registra:

resultado obtido;
avaliação;
falha;
ajuste recomendado.

Exemplo:

T01 — v1

Falha:
inventou métricas inexistentes.

Ajuste:
proibir criação de números sem evidência.

↓
criação da v2

O campo changeReason pode existir como um resumo opcional, mas o funcionamento do versionamento não deve depender dele.

A rastreabilidade principal deve permanecer no histórico dos testes.

12. Reteste

Criar uma nova versão não significa automaticamente que o prompt ficou melhor.

Toda nova versão deve ser testada.

Sempre que possível, utilizar:

a mesma entrada;
os mesmos critérios de avaliação.

Exemplo:

v1 + Entrada A
↓
T01
↓
reprovado

alteração

v2 + Entrada A
↓
T02
↓
aprovado

Somente a comparação entre os testes permite demonstrar que houve melhoria.

13. Fluxo completo
CRIAR PROMPT
      ↓
   RASCUNHO
      ↓
pronto para testar
      ↓
      v1
      ↓
    TESTE
      ↓
   AVALIAÇÃO
      ↓
┌───────────────────────────────────────┐
│                                       │
APROVADO                       AJUSTAR / REPROVADO
│                                       │
mantém versão atual             registrar falha
│                                       ↓
│                              registrar ajuste
│                                       ↓
│                             fazer alteração
│                                       ↓
│                         altera comportamento?
│                              ↙             ↘
│                            NÃO             SIM
│                             ↓               ↓
│                    ajuste editorial     nova versão
│                     mesma versão            ↓
│                             │             RETESTE
│                             │                ↓
└─────────────────────────────┴────────→ HISTÓRICO
14. Exemplo completo
OP-02 — v1

É executado o teste T01.

Resultado:

O prompt inventou métricas que não estavam presentes na entrada.

Avaliação:

reprovado

Falha:

Criação de números sem evidência.

Ajuste recomendado:

Proibir métricas não sustentadas pela entrada.

Como a alteração modifica o comportamento esperado do prompt, é criada:

OP-02 — v2

Depois é executado o T02, preferencialmente utilizando a mesma entrada e os mesmos critérios.

Resultado:

O prompt não inventou métricas.

Avaliação:

aprovado

Histórico:

OP-02
│
├── v1
│   └── T01 ❌ reprovado
│
└── v2 ← atual
    └── T02 ✅ aprovado

Se posteriormente houver apenas uma correção como:

"Analize" → "Analise"

a versão continua sendo v2.

A alteração fica registrada em editorialChanges e o textSnapshot dos testes anteriores continua preservando exatamente o texto utilizado em cada execução.

15. Resumo das regras
ID	Regra
RV-01	O prompt permanece como rascunho até ficar pronto para o primeiro teste; então nasce a v1.
RV-02	Depois de testada, uma versão entra no histórico e não pode ser sobrescrita por alterações operacionais.
RV-03	Todo teste mantém vínculo permanente com a versão realmente executada por meio de versionId.
RV-04	O teste registra a execução completa e preserva em textSnapshot o texto exato utilizado.
RV-05	A avaliação utiliza aprovado, ajustar ou reprovado.
RV-06	Mudança capaz de alterar comportamento, interpretação, formato ou resposta da IA cria nova versão.
RV-07	Mudança claramente editorial mantém a versão e é registrada em editorialChanges.
RV-08	Em caso de dúvida sobre o impacto da alteração, criar nova versão.
RV-09	currentVersionId identifica a única versão atual, preservando as anteriores.
RV-10	A justificativa principal da nova versão permanece no teste anterior; changeReason é apenas opcional.
RV-11	Toda nova versão precisa ser retestada.
RV-12	A melhoria é comprovada pela comparação dos testes vinculados às respectivas versões.
16. Integração com o modelo de dados

A proposta utiliza os seguintes campos do modelo de dados:

Prompt

currentVersionId

Identifica a versão atualmente ativa.

Version

editorialChanges

Preserva alterações editoriais realizadas sem criar nova versão.

changeReason

Pode permanecer opcional como resumo da alteração.

Test

versionId

Mantém o vínculo permanente entre o teste e a versão executada.

textSnapshot

Preserva exatamente o texto utilizado no momento daquele teste.

17. Status da proposta

Esta especificação está preparada para revisão na Etapa 0.

Ela não deve ser considerada oficialmente aprovada até a validação da equipe e de Rogério.
