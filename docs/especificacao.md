# Especificação do esqueleto de programa de governança

## Objetivo

Uma ferramenta genérica e **replicável** para implantar programas de governança e SGSI em empresas de
tecnologia e desenvolvimento de software, porte pequeno a médio, funções
executivas acumuladas. A implantação entrega documentos coesos e prontos para aprovação em dias, com as decisões de
calibragem feitas por entrevista.

## Decisões de desenho (13/11/2026)

| Tema | Decisão |
| --- | --- |
| Público | Empresas de tecnologia e software; não necessariamente de segurança |
| Onde o programa roda | Só documentos; o cliente opera com as próprias ferramentas |
| Formato de entrega | Um repositório Git por cliente, com o gerador e a conferência de coesão |
| Entrevista | Skill de entrevista no repositório; um consultor valida antes da primeira emissão |
| Origem do texto | Acervo real em uso, generalizado: sem nomes, marcas, ferramentas ou decisões de uma empresa |

## Fora do escopo

- Operação do programa (registro de riscos, revisão de acessos e indicadores em sistema): o cliente usa as ferramentas
  dele; o esqueleto entrega modelos de planilha.
- Integração com sistemas de gestão de terceiros (GRC, painéis, ITSM).
- Declaração de Aplicabilidade (SoA): entra numa versão posterior.
- Preço e forma de venda da oferta (decisão comercial).

## 1. Repositório modelo

Um repositório template no GitHub; cada cliente nasce de um clone e mantém o modelo como remoto `esqueleto`.

```
config/empresa.yaml      parâmetros da empresa (única fonte do que é específico)
normas/                  texto fixo das normas, com marcadores e trechos por módulo
procedimentos/           procedimentos em texto (papéis, passos e prazos)
modelos/                 planilhas: registro de riscos, revisão de acessos, exceções, indicadores, decisões
decisoes/                registro de decisões do cliente
acrescimos/              texto próprio do cliente, anexado às normas sem editá-las
ferramentas/             gerador (HTML A4 → PDF e Word), conferência de coesão, verificação de links
.claude/skills/entrevista-governanca/   a entrevista
.github/workflows/       conferência e geração em toda PR; publicação dos PDFs na versão
exemplo/nclient.yaml     empresa fictícia usada nos testes do modelo
```

### Normas

| Conjunto | Documentos |
| --- | --- |
| Núcleo (sempre) | Política de segurança e privacidade, Governança e riscos, Ética e conduta, Acesso e ativos, Fornecedores, Pessoas, Guia do colaborador |
| Módulo desenvolvimento | Desenvolvimento seguro, mudanças e projetos |
| Módulo serviço ao cliente | Ativos e dados de clientes, sem o conteúdo específico de segurança gerenciada (SOC, firewall como serviço, cadeia de custódia forense) |
| Módulo continuidade | Incidentes e continuidade, sem a dependência de SOC |

A generalização remove o que é de uma empresa: nomes, marcas, ferramentas internas e decisões datadas. A estrutura, o glossário, a tabela de unicidade e a hierarquia documental (política e governança como pilares)
são mantidos.

## 2. Parametrização

- O texto das normas não é editado pelo cliente; tudo que é específico está em `config/empresa.yaml`.
- Marcadores `{{caminho.do.parametro}}` no texto e blocos condicionais por módulo (`{{#modulo.desenvolvimento}} ...
  {{/modulo.desenvolvimento}}`).
- Grupos de parâmetros: empresa (razão social, marcas, porte, faturamento como referência relativa), módulos, cargos e
  pessoas (Anexo A, com substitutos), instâncias (comitês ativos), alçadas e escala de impacto em percentual do
  faturamento, tolerância e apetite, ferramentas por finalidade, prazos (revisão de acessos, retenção, correção de
  vulnerabilidades, notificação de incidente) e identidade visual (logotipo, cor de destaque, fontes).
- **Parâmetro sem resposta não bloqueia a emissão:** o trecho sai com a recomendação padrão e o item entra
  automaticamente em "Pontos a decidir" da norma, com a recomendação. A recomendação padrão vem da calibragem do acervo
  de referência, revisada para o perfil de empresa de tecnologia.
- Texto próprio do cliente entra em `acrescimos/<norma>.md`, renderizado ao fim da seção indicada, sem tocar no texto
  fixo.

## 3. Entrevista

Skill `entrevista-governanca` no repositório, conduzida com o cliente (ou pelo consultor junto dele):

1. Ordem: empresa e porte → módulos → cargos, acúmulos e substitutos → instâncias → alçadas → escala de risco e
   tolerância → ferramentas por finalidade → prazos → identidade visual.
2. Uma pergunta por vez, sempre com a recomendação e o motivo; respostas curtas.
3. Cada resposta grava o parâmetro em `config/empresa.yaml` e uma linha datada em `decisoes/`.
4. Ao fim, gera as normas, roda a conferência e abre uma PR.
5. O consultor revisa a PR (coerência das decisões com o porte e o setor, segregação de funções); só depois do merge sai
   a primeira emissão.

## 4. Emissão e manutenção

- `ferramentas/gerar` emite HTML A4, PDF e Word com capa, controle de quebras, figuras SVG e a identidade visual do
  cliente (padrão neutro quando não informada; tema completo por tokens em `config/tema/`, ver
  [identidade-visual.md](identidade-visual.md)).
- O modelo tem versão (SemVer). O cliente recebe melhorias com `git merge esqueleto/main`; como o específico fica em
  `config/` e `acrescimos/`, o conflito é raro. Notas de versão dizem o que muda em cada norma.
- Revisão anual e aprovação seguem a norma de governança do próprio cliente (aprovação pela direção, por PR).

## 5. Qualidade

- `ferramentas/coesao`: glossário, estrutura padrão, pessoas só no Anexo A,
  pontos a decidir com recomendação, exceções pelo processo, **nenhum marcador sem valor nem sem recomendação padrão**.
- CI do modelo: gera todas as normas para `exemplo/nclient.yaml` com todos os módulos e com só o núcleo; falha com
  marcador sem resolução, coesão diferente de zero, link quebrado ou tabela partida.
- CI do cliente: a mesma conferência em toda PR.

## 6. Entregas (para o plano)

1. Repositório modelo com a estrutura acima e o gerador.
2. Extração e generalização das normas do núcleo e dos três módulos, com marcadores.
3. Esquema e valores padrão de `config/empresa.yaml`, e a empresa exemplo.
4. Procedimentos e modelos de planilha.
5. Skill de entrevista.
6. Conferência de coesão adaptada e CI.
7. Piloto com uma empresa real do perfil.
