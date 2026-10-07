# Cronograma de Aplicação & Roadmap Técnico — Wibag Systems

> Transcrição do PDF "Cronograma_Aplicacao_Wibag_System" (emissão 30/09/2026 10:13).
> Fonte de verdade para prazos. Em caso de divergência com o Panorama, registrar a divergência em vez de escolher um lado.

## Premissa operacional
- Dedicação compartilhada (em paralelo com a rotina operacional da empresa).
- Buffers de segurança de 20% a 30%.
- Ciclos de entrega quinzenais, sem comprometer o suporte às Wibags em campo.

## 1. Visão geral das fases

| Fase | Escopo principal | Prazo estimado | Regime |
|---|---|---|---|
| Fase 1 | Repositórios GitHub, contas Claude/Agentes, plugins, portas, ambientes Prod/Homolog | 2 a 3 semanas | Estrutural / Fundação |
| Fase 2 | Salvaguarda de bancos, refatoração do legado, pesquisas técnicas & scaffolding | 3 a 4 semanas | Consolidação & Segurança |
| Fase 3 | Programação ativa dos 8 módulos, integrações ponta a ponta & entregas | 8 a 12 semanas | Sprints quinzenais |
| **Total** | Ecossistema 100% versionado, integrado e em produção estável | **13 a 19 semanas (~3 a 4,5 meses)** | Evolução contínua |

## 2. Fase 1 — Fundação, ferramental & infraestrutura (semanas 1–3)

### 2.1 Governança de repositórios GitHub & contas (semana 1)
- Repositórios oficiais: `wibag-hub`, `wibag-core`, `wibag-estoque`, `wibag-manutencao`, `wibag-monitoramento`, `wibag-report`, `wibag-agenda`, `wibag-docs`.
- Contas Claude / AI Agents com leitura/escrita e chaves SSH (`client_public.key`).
- Branches: `main` (produção estável), `staging` (homologação pré-entrega), `feature/*` (desenvolvimento isolado).

### 2.2 Ferramental, plugins & orquestração entre agentes (semana 2)
- Plugins: análise de vulnerabilidades, linters PHP/JS, geradores de testes de regressão.
- Arquivos de regras (`.agents/rules`) com a arquitetura das Wibags.
- Protocolo de handoff: levantamento de requisitos → codificação orientada a testes → code review.

### 2.3 Mapeamento de portas e segregação de ambientes (semana 3)
- Portas ativas no servidor Linux: MySQL 3306, Apache/Nginx 80/443, daemons 8080, 8443, 28082.
- Homologação: porta 8081 (Portal Staging) e schema isolado `wibag_staging` no MySQL.
- Virtual Hosts protegidos com isolamento de sessão.

## 3. Fase 2 — Salvaguarda de dados, legado & base dos novos projetos (semanas 4–7)

### 3.1 Salvaguarda dos bancos & rollback (semana 4)
- Snapshot completo e dumps estruturados em `/home/tomich/backups/`.
- Homologação do DDL unificado `migracao_consolidada_v1.sql` com script de rollback testado.
- Base espelho sanitizada para desenvolvimento (dados de clientes e chips protegidos).

### 3.2 Modernização e blindagem do legado (semana 5)
- Queries vulneráveis → PDO com Prepared Statements.
- Sessão corporativa unificada e permissões por perfil (Admin vs. Técnico) via `auth_shared.php`.
- Preservar regras de negócio já validadas (máquina de estados dos chips e históricos).

### 3.3 Pesquisas técnicas, scaffolding & design system (semanas 6–7)
- Migração da telemetria MikroTik legada para serviços systemd.
- PoC de Speedtest CLI integrado ao FPDF para laudos.
- Design System: Glassmorphism corporativo com tema escuro/claro.

## 4. Fase 3 — Desenvolvimento ativo, sprints quinzenais (semanas 8–19)

| Sprint | Semanas | Entregáveis | Impacto operacional |
|---|---|---|---|
| Sprint 1 | 8 e 9 | **Estoque por Lotes (3NF)**: interface administrativa completa, migração DDL e card ativo no Portal Hub | Rastreabilidade total de insumos e estoque mínimo por categoria sem planilhas externas |
| Sprint 2 | 10 e 11 | **Integração Estoque ↔ Manutenção**: consumo de peças por lote vinculado à OS da Wibag | Baixa automática do saldo ao reparar uma maleta e histórico de peças por patrimônio |
| Sprint 3 | 12 e 13 | **Agenda de Eventos Inteligente**: bloqueio automático de Wibags com falhas e conferência de chips ativos | Zero envio de equipamento com pendência técnica ou sem conectividade |
| Sprint 4 | 14 e 15 | Modernização MikroTik: daemon systemd + bot de alertas (WhatsApp/TG) | Avisos imediatos se operadora cair no evento; histórico de latência/banda |
| Sprint 5 | 16 e 17 | Laudos com Speedtest: medição automática e PDF no histórico da Wibag | Comprovação técnica para clientes |
| Sprint 6 | 18 e 19 | Governança & Go-Live: 2FA, auditoria geral, virada para `main` | Ecossistema blindado e auditável |

## Diretrizes de comunicação
- Status report semanal (progresso + próximos passos).
- Sprint Review quinzenal em Homologação antes de qualquer publicação em Produção.
