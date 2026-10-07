# Panorama Técnico & Roadmap Evolutivo — Wibag Systems

> Transcrição do PDF "Panorama_Projetos_Wibag_System" (emissão 29/09/2026 10:11).
> Fonte de verdade para o estado atual dos módulos.

## Contexto do ecossistema
O Wibag System centraliza a infraestrutura operacional de maletas de conectividade 4G/5G para transmissões ao vivo: inventário físico, telemetria de rede, manutenção preventiva, laudos de bancada e logística de eventos.

## 1. Panorama por módulo

### 1. Sistema Wibag (Controle de Wibags e Chips) — OPERACIONAL
- **Finalidade:** gestão do parque de mochilas físicas e do inventário de SIM cards (Vivo, Claro, TIM etc.).
- **Feito:** CRUD de Wibags e Chips; máquina de estados dos chips (`atribuído`, `livre`, `vago`, `sumiu`); histórico cronológico auditável de movimentações; interface com tooltips e modais.
- **Próximos passos:** automatizar atualização de status com a Agenda de Eventos; alertas para chips "sumidos" ou sem uso por longos períodos.

### 2. Monitoramento Geral (Telemetria MikroTik) — OPERACIONAL / NÚCLEO ATIVO
- **Finalidade:** status, consumo de banda e latência dos roteadores MikroTik das Wibags.
- **Feito:** modelagem (telemetria e interfaces); collectors e daemons/watchdogs; painel de tráfego.
- **Próximos passos:** migrar daemons batch/VBS para systemd ou Docker; alertas via Telegram/WhatsApp.

### 3. Gestão de Equipamentos e Manutenção — OPERACIONAL / EM EXPANSÃO
- **Finalidade:** prontuário técnico de cada Wibag, manutenções corretivas/preventivas e pendências de bancada.
- **Feito:** ordens de manutenção; pendências por gravidade; **integração recente com a API de Estoque (consumo de materiais por lote na OS)**.
- **Próximos passos:** checklists digitais para revisão pré-evento; cálculo automático de MTBF para antecipar compras de reposição.

### 4. Gerador de Testes e Laudos (Wibag Report) — OPERACIONAL
- **Finalidade:** homologação de entrega e laudos técnicos para clientes.
- **Feito:** registro de ocorrências com fotos e gráficos; motor de PDF.
- **Próximos passos:** Speedtest CLI e gravação do laudo no histórico patrimonial da Wibag.

### 5. Agenda de Eventos — OPERACIONAL
- **Finalidade:** reservas e alocação de Wibags para eventos contratados.
- **Feito:** calendário visual; alocação de equipamentos por data; controle de status de entrega.
- **Próximos passos:** **travar automaticamente a reserva de Wibags com manutenção pendente ou chips faltantes; notificar equipe sobre desmobilização.**

### 6. Sistema de Gestão de Estoque por Lote — EM AJUSTES E CONSOLIDAÇÃO
- **Finalidade:** rastreabilidade de insumos, peças de reposição e componentes em 4 níveis (**Categoria → Item → Modelo → Lote**).
- **Feito:** especificação técnica 3NF; **estoque mínimo calculado na categoria**; script DDL unificado `migracao_consolidada_v1.sql` integrando estoque ao `wibag_system`.
- **Próximos passos:** finalizar telas administrativas da arquitetura de lotes; habilitar o card no Portal Central; liberar acesso aos operadores do almoxarifado.

### 7. Portal Central Hub & Governança (SSO) — OPERACIONAL
- **Feito:** interface Glassmorphism; autenticação centralizada (`auth_shared.php`); monitor do servidor; perfis Admin / Operador.
- **Próximos passos:** 2FA; dashboard gerencial com KPIs dos demais sistemas.

### 8. Documentação Interna & Base de Conhecimento — OPERACIONAL
- **Feito:** centralização de ajuda e tutoriais técnicos.
- **Próximos passos:** FAQ indexado; versionamento de POPs.

## 2. Matriz de melhorias por horizonte

| Horizonte | Ações | Benefício |
|---|---|---|
| Curto prazo (1 a 3 semanas) | Ativar nova interface de Estoque; aplicar migração DDL consolidada; validar baixa de peças na Manutenção | Almoxarifado em operação total e rastreabilidade de peças por Wibag |
| Médio prazo (1 a 2 meses) | Sincronização automática Eventos ↔ Wibags; daemons MikroTik para Linux; robô de alertas Telegram/WhatsApp | Impedir agendamento de Wibags defeituosas; avisos em tempo real |
| Longo prazo (3 a 6 meses) | PWA/App para técnicos de campo; BI com custos por operadora; Speedtest automático no laudo | Operação sem papel, laudos assinados no local, controle financeiro |

## 3. Síntese executiva
O momento é de consolidação: integrar as pontas já construídas (**Estoque ↔ Manutenção ↔ Eventos**) e enriquecer a telemetria com automações ativas.
