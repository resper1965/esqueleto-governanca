# Parâmetros

Referência do `config/empresa.yaml`, o único arquivo que diz o que é específico da empresa.

> [!NOTE]
> Rascunho do esquema. A versão definitiva sai com a entrega 3 do [roteiro](../README.md#roteiro), junto da validação
> automática.

## Exemplo completo

Empresa fictícia `nClient`, com todos os módulos:

```yaml
empresa:
  razao_social: nClient Tecnologia Ltda.
  marcas: [nclient]
  colaboradores: 45
  faturamento_referencia: 12000000     # R$ por ano; base das alçadas e da escala de risco

modulos:
  desenvolvimento: true
  servico_ao_cliente: true
  continuidade: true

cargos:
  ceo:            { titulo: Diretor-presidente, pessoa: A. Usuário,  substituto: cfo }
  cfo:            { titulo: Diretor financeiro, pessoa: B. Usuário,  substituto: ceo }
  responsavel_sgsi: { titulo: Responsável pelo SGSI, pessoa: C. Usuário, substituto: lider_tecnico }
  encarregado:    { titulo: Encarregado (DPO),  pessoa: D. Usuário,  substituto: responsavel_sgsi }
  lider_tecnico:  { titulo: Líder técnico,      pessoa: E. Usuário,  substituto: cto }
  cto:            { titulo: Diretor de tecnologia, pessoa: E. Usuário, substituto: ceo }
  acumulos:
    - cargos: [lider_tecnico, cto]
      controle_compensatorio: Mudanças em produção aprovadas também pelo responsável pelo SGSI

instancias:
  comite_riscos:   { frequencia: trimestral }
  comite_mudancas: { ativo: true, frequencia: semanal }
  comite_crise:    { decide: ceo }

alcadas:                               # frações do faturamento de referência
  contratacao:
    gestor_limite: 0.01
    cfo_limite: 0.05                   # acima disso, ceo
  aceite_risco:
    medio: lider_area
    alto: cfo
    critico: ceo

risco:
  escala_impacto:                      # perda estimada / faturamento
    baixo: 0.001
    medio: 0.005
    alto: 0.02
    critico: 0.05
  apetite: medio

ferramentas:
  chamados_internos: Jira Service Management
  projetos_internos: Jira
  projetos_cliente: GLPI
  identidade: Entra ID
  registro_riscos: modelos/riscos.xlsx

prazos:
  revisao_acessos: trimestral
  revisao_acessos_privilegiados: mensal
  vulnerabilidade_critica_dias: 7
  notificacao_incidente_horas: 24
  retencao_logs_meses: 12

identidade:                            # opcional; ver docs/identidade-visual.md
  logotipo: config/logotipo.svg
  cor_destaque: "#2563EB"
  fonte_titulos: Montserrat
  fonte_texto: Inter
  # tema: config/tema/tokens.json      # design system completo, no lugar dos campos acima
```

## Grupos

| Grupo | Obrigatório | Padrão quando vazio | Usado em |
| --- | --- | --- | --- |
| `empresa` | razão social e faturamento | — | Capa, alçadas, escala de risco |
| `modulos` | não | todos `false` (só o núcleo) | Seleção de normas e blocos |
| `cargos` | ceo e responsavel_sgsi | papel acumulado pelo ceo, com aviso | Anexo de designações, papéis de todas as normas |
| `instancias` | não | só comitê de riscos, trimestral | Governança e riscos |
| `alcadas` | não | calibragem de origem | Governança, Fornecedores |
| `risco` | não | escala de origem, apetite médio | Governança e riscos |
| `ferramentas` | não | "a definir" + ponto a decidir | Todas as normas |
| `prazos` | não | calibragem de origem | Acesso, Desenvolvimento, Continuidade |
| `identidade` | não | padrão neutro | Gerador ([identidade visual](identidade-visual.md)) |

## Regras de validação previstas

- Cargo citado em `substituto`, `alcadas` ou `acumulos` precisa existir em `cargos`.
- Ninguém é o próprio substituto.
- Acúmulo sem `controle_compensatorio` reprova.
- Frações de alçada crescentes (`gestor_limite` < `cfo_limite`).
- Escala de impacto estritamente crescente.
- Pessoas aparecem só em `cargos`; nenhuma norma cita nome.
