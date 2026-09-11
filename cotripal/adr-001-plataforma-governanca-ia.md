# ADR-001: Arquitetura da Plataforma de Governança de IA — Cotripal

**Status:** Proposto (aguardando decisão de KNova)
**Data:** 2026-09-11
**Decisor:** KNova (PM de Inovação / Tech Lead, Cotripal)

---

## Contexto

A Cotripal precisa formalizar o processo de governança de uso de IA (intake → qualificação de risco → aprovação → acompanhamento). Já existem quatro peças de infraestrutura relevantes na empresa:

| Ferramenta | Papel hoje |
|---|---|
| **Aevo** | Kanban onde gestores cadastram e acompanham iniciativas — fonte de verdade de acompanhamento |
| **Nextcloud + Forms** | Drive interno e captura simples de formulários |
| **Fluig (BPM)** | Cogitado para orquestrar o fluxo do comitê |
| **Agente de IA → CRM** | Coleta dados e grava JSON em banco — pipeline de captura já validado |

Duas soluções vibe-coded em Base44 (gestão de terceiros, gestão de iniciativas) já foram construídas fora desse ecossistema e estão paradas aguardando aprovação da TI — sem integração com ERP, base de usuários, banco padrão ou Git da empresa. Isso é evidência direta do risco de construir mais uma plataforma desconectada.

## Decisão

**Não construir um sistema de gestão de iniciativas paralelo ao Aevo.** Construir apenas a camada de lógica de governança que não existe em nenhuma ferramenta atual, e integrar o resultado ao Aevo como etapa/gate do fluxo já existente.

### O que a nova plataforma resolve (não existe hoje em nenhum lugar)
- Formulário estruturado de qualificação de IA com lógica condicional
- Matriz de classificação de risco em 4 dimensões, com cálculo/roteamento automático
- Regras de alçada de aprovação por nível de risco
- Portal único de materiais educativos (módulo de treinamento, checklist de avaliação técnica)
- **Camada de validação de segurança/código** (ver seção dedicada abaixo — adicionada nesta revisão)

### O que permanece nas ferramentas existentes
- Acompanhamento de iniciativas e kanban → **Aevo** (integração via API/webhook, não substituição)
- Armazenamento de arquivos e captura de formulários simples → **Nextcloud**
- Orquestração de processo/BPM, se necessário → **Fluig**
- Telemetria de shadow AI → reaproveitar pipeline do **agente de IA → CRM/JSON** já existente, se aplicável

## Consequências

**Positivas**
- Evita competir com o Aevo pela "fonte da verdade" de iniciativas — reduz o mesmo risco que travou as plataformas Base44
- Superfície de manutenção menor: só se constrói o que é realmente novo
- Caminho de aprovação pela TI mais simples, por já nascer integrado ao ecossistema aprovado

**Negativas / trade-offs**
- Depende de a Aevo ter API ou mecanismo de integração viável (a confirmar tecnicamente)
- Introduz uma dependência de integração a mais para manter
- Exige alinhamento com times de Aevo/Fluig antes de travar o desenho técnico

**Pendente de validação técnica**
- Aevo expõe API? Webhook? Só importação manual?
- Fluig realmente entra no fluxo do comitê ou fica fora do escopo v1?

---

## Seção adicionada: Validação de Segurança e Código-Fonte

### O problema
KNova aprova arquitetura, processo e stack — mas segurança e revisão de código estão fora de sua expertise, e a Cotripal não tem ninguém internamente com esse perfil. O caminho atual (pentest terceirizado por solução) é economicamente inviável para escalar a quantidade de soluções vibe-coded que passam pela aprovação dela.

Isso não é uma preocupação paralela ao processo de governança — **é uma das dimensões da matriz de risco** e precisa de um mecanismo de validação que não dependa 100% de terceirização.

### Direções a avaliar (não decidido — para discussão técnica futura)

1. **SAST (Static Application Security Testing) automatizado**
   Ferramentas open source (ex: Semgrep, Bandit para Python, gosec para Go) rodando como gate automático antes de uma solução avançar no fluxo de aprovação — reduz o volume que precisa de revisão humana.

2. **Scanners de vulnerabilidade de dependências**
   Ferramentas como OWASP Dependency-Check ou Trivy identificam bibliotecas com CVEs conhecidas — relevante porque soluções vibe-coded tendem a acumular dependências sem curadoria.

3. **LLM como camada de triagem de código**
   Usar um modelo (incluindo Claude) como primeira triagem de código-fonte antes de decidir se uma solução precisa de pentest humano/terceirizado — não substitui pentest para casos de alto risco, mas filtra o volume.

4. **Pentest terceirizado permanece, mas só para o topo da matriz de risco**
   Reservar o orçamento de terceirização para as iniciativas classificadas como alto risco pela matriz — não para todas.

### Próximo passo desta seção
Levantar, com KNova, quais dessas ferramentas já têm precedente de uso na Cotripal ou no ecossistema do Instituto Agregar, e desenhar como a "aprovação de segurança" se encaixa como uma etapa formal (não um gate manual ad-hoc) dentro do fluxo do comitê.

---

## Próximos passos

1. Confirmar com time técnico se Aevo expõe API/webhook
2. Detalhar a matriz de risco em 4 dimensões (uma delas passa a ser explicitamente "risco de segurança/código")
3. RFC de arquitetura técnica da camada nova (frontend → specs → backend)
4. Levantamento de ferramentas SAST/scanning como spike técnico separado
