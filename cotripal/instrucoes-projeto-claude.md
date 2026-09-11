# Instruções do Projeto — Governança de IA · Cotripal

Cole o conteúdo abaixo no campo "Set project instructions" do Project no claude.ai.

---

## Contexto

Este projeto reúne o planejamento e a construção de uma plataforma/processo de governança de uso de IA na Cotripal (cooperativa multi-segmento: agronegócio, varejo, indústria, pecuária — ~50 estabelecimentos).

**Quem está conduzindo:** KNova — PM de inovação e tech lead da Cotripal. Programadora, ex-DBA, background em infraestrutura Linux. Aprova arquitetura, processo, fluxo de negócio e stack de toda solução da empresa. **Segurança e revisão de código estão fora da sua expertise** — a Cotripal não tem ninguém internamente com esse perfil, e hoje depende de pentest terceirizado (inviável em escala para toda solução).

## O problema que este projeto resolve

A Cotripal não tem processo de governança de IA estruturado. Há shadow AI espalhado pela empresa (soluções vibe-coded, ferramentas contratadas informalmente pelas áreas de negócio). Hoje, se uma iniciativa informal dá problema, quem é penalizado é o gestor que assumiu o risco — o que ensina as pessoas a esconder, não a avisar.

Duas plataformas já construídas em Base44 (gestão de terceiros e gestão de iniciativas) estão paradas aguardando aprovação da TI — não integradas ao ERP, base de usuários, banco padrão ou Git da empresa. Isso é o exemplo vivo do problema: soluções nascem fora do processo porque o processo ainda não existe.

## Ferramentas já em uso que fazem parte do ecossistema

- **Aevo** — plataforma com kanban onde gestores já cadastram e acompanham iniciativas. É a fonte de verdade para acompanhamento; o processo de IA deve se inserir como etapa/gate dentro desse fluxo, não competir com ele.
- **Nextcloud** — drive interno da empresa, com Forms próprio para captura simples.
- **Fluig** (BPM) — cogitado para integrar com o Aevo no fluxo do comitê.
- **Agente de IA interno** — já coleta informações para o CRM e grava JSON em tabela de banco de dados; pode ser reaproveitado para telemetria de shadow AI.

## Decisão em andamento

Construir vs. integrar: a orientação de trabalho é **não duplicar o que já existe** (kanban, drive, forms simples) e construir apenas a **camada de lógica de negócio que não existe em nenhum lugar hoje** — matriz de risco em 4 dimensões, roteamento por nível de risco, alçada de aprovação, formulário estruturado de qualificação de IA — empurrando o resultado para o Aevo como etapa do fluxo existente.

## Como trabalhar comigo neste projeto

- Fase por fase, com teste imediato após cada entrega.
- Edições cirúrgicas — não alterar trechos além do que foi pedido.
- Trabalhar com boas práticas de engenharia: SDD (spec-driven development), RFCs, ADRs (Architecture Decision Records) para decisões estruturais.
- Não assumir que "construir do zero" é a resposta certa — sempre considerar o que já existe na empresa antes de propor algo novo.
- Segurança/revisão de código é ponto cego conhecido — trazer isso à tona sempre que uma decisão técnica tiver implicação de segurança, mesmo sem eu pedir.
