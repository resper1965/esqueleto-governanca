# Glossário e unicidade

Duas regras que mantêm o acervo coeso: **um termo por conceito** e **um lugar por regra**. A conferência de coesão
reprova o que fugir delas.

## Glossário

| Termo usado | Não usar | Definição |
| --- | --- | --- |
| colaborador | funcionário, empregado | Toda pessoa que trabalha para a empresa, CLT, PJ ou estagiário |
| gestor | gestor imediato, chefe, superior | Quem responde pela pessoa ou pela área |
| responsável pelo SGSI | CISO, gestor de segurança | Quem coordena o sistema de gestão de segurança da informação |
| encarregado (DPO) | DPO isolado, data protection officer | Encarregado pelo tratamento de dados pessoais (LGPD, art. 41) |
| dono do ativo | proprietário, owner | Quem responde pela classificação e pelo acesso a um ativo |
| terceiro | parceiro, prestador (como termo geral) | Fornecedor, prestador ou parceiro com acesso a ativo ou dado |
| exceção | waiver, desvio aceito | Descumprimento temporário autorizado pelo processo único de exceções |
| aceite de risco | risco aceito informalmente | Decisão registrada de manter um risco, pela alçada definida |
| mudança | change, alteração em produção | Alteração em ambiente produtivo, registrada e aprovada |
| incidente | ocorrência, problema | Evento que compromete ou pode comprometer confidencialidade, integridade ou disponibilidade |
| alinhada aos requisitos | certificada, em conformidade com | Forma de citar ISO/IEC 27001, 27701 e LGPD |

## Onde mora cada regra

A regra fica num documento; os demais apontam para ele.

| Regra | Mora em | Os outros dizem |
| --- | --- | --- |
| Quem aprova normas | Governança e riscos | "conforme a norma de governança" |
| Alçadas de aprovação | Governança e riscos | "pela alçada da norma de governança" |
| Processo de exceção | Governança e riscos | "pelo processo de exceções" |
| Método de risco e aceite | Governança e riscos | "avaliado pelo método de riscos" |
| Classificação da informação | Política | "conforme a classificação da Política" |
| Concessão, revisão e revogação de acesso | Acesso e ativos | "pelo ciclo de acesso" |
| Entrada e saída de pessoas | Pessoas | "na jornada de entrada (ou saída)" |
| Avaliação de terceiros | Fornecedores e terceiros | "homologado pela norma de terceiros" |
| Resposta a incidente | Incidentes e continuidade | "registrado como incidente" |
| Gestão de mudanças | Desenvolvimento seguro | "por GMUD" |
| Dados e ambientes de cliente | Serviço ao cliente | "conforme a norma de serviço ao cliente" |
| Nomes de pessoas | Anexo de designações da governança | Nenhuma norma cita nome |

## Estrutura padrão de uma norma

```text
1. Objetivo
2. Escopo
3. Papéis
4..n. Regras (seções numeradas, IDs estáveis)
   Exceções e medição
   Revisão
Pontos a decidir         (gerado; some quando vazio)
Aprovação                (gerado a partir de decisoes/)
```

Os IDs de seção são estáveis entre versões: seção nova recebe número novo, nunca reaproveita um existente.
