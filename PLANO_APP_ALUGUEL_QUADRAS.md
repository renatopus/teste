# Plano de Construção — App de Aluguel de Quadras com Agendamento

## 1) Objetivo
Construir um aplicativo para gerenciamento e aluguel de quadras esportivas com foco em:
- descoberta de quadras;
- agendamento de horários;
- pagamento;
- operação do proprietário/administrador.

Como você já possui as telas e o modelo de negócio, o foco aqui é transformar isso em um plano técnico executável.

## 2) Escopo do MVP

### Funcionalidades para cliente final
1. Cadastro e login (e-mail/senha + social opcional).
2. Busca de quadras por localização, esporte e faixa de preço.
3. Visualização de disponibilidade por data/horário.
4. Reserva de horário com confirmação imediata.
5. Pagamento online (PIX/cartão) e comprovante.
6. Histórico de reservas e opção de cancelamento conforme regra.

### Funcionalidades para dono de quadra / operador
1. Cadastro de arena e quadras (tipos, preço, fotos, regras).
2. Definição de grade de horários disponíveis.
3. Bloqueio manual de horários (manutenção/eventos).
4. Painel de reservas do dia e gestão de clientes.
5. Relatórios básicos (ocupação, faturamento, cancelamentos).

## 3) Arquitetura recomendada

### Front-end
- **Mobile**: Flutter ou React Native (um código para Android/iOS).
- **Web administrativo**: React + Next.js.

### Back-end
- **API**: Node.js (NestJS) ou Python (FastAPI).
- **Banco relacional**: PostgreSQL.
- **Cache/fila (opcional para escala)**: Redis.
- **Armazenamento de imagens**: S3 compatível.

### Integrações
- Gateway de pagamento (ex.: Stripe, Mercado Pago, Pagar.me).
- Notificações push e e-mail transacional.
- Geolocalização/mapas (Google Maps/Mapbox).

## 4) Modelo de domínio (entidades principais)
- **Usuário**: perfil, tipo (`cliente`, `operador`, `admin`).
- **Arena**: estabelecimento com dados gerais.
- **Quadra**: pertence à arena, contém esporte, preço e capacidade.
- **AgendaDisponibilidade**: janelas de horário padrão.
- **BloqueioAgenda**: indisponibilidades específicas.
- **Reserva**: cliente + quadra + intervalo + status.
- **Pagamento**: transação associada à reserva.
- **Cupom** (opcional para marketing).
- **Avaliação** (opcional para reputação).

## 5) Regras de negócio críticas
1. **Evitar overbooking**: não pode haver sobreposição de reservas na mesma quadra.
2. **Preço por faixa horária**: permitir valor diferente para pico/vale.
3. **Política de cancelamento**: parametrizável por arena.
4. **Confirmação condicionada ao pagamento** (quando aplicável).
5. **Expiração de reserva pendente**: liberar horário automaticamente após X minutos.

## 6) API inicial (exemplo)
- `POST /auth/register`
- `POST /auth/login`
- `GET /arenas`
- `GET /arenas/{id}/courts`
- `GET /courts/{id}/availability?date=YYYY-MM-DD`
- `POST /bookings`
- `GET /bookings/me`
- `POST /payments/create-intent`
- `POST /webhooks/payment`
- `PATCH /operator/bookings/{id}/status`

## 7) Fluxo de agendamento (alto nível)
1. Usuário seleciona quadra + data + horário.
2. Backend valida disponibilidade em transação.
3. Cria reserva em status `pending_payment`.
4. Cria intenção de pagamento.
5. Webhook confirma pagamento e altera para `confirmed`.
6. Envia notificação para cliente e operador.

## 8) Roadmap de execução (8 semanas)

### Semana 1-2
- Setup do repositório, CI/CD, autenticação e perfis.
- Cadastro de arena/quadra e upload de mídia.

### Semana 3-4
- Grade de disponibilidade + bloqueios.
- Busca de quadras + filtros.

### Semana 5-6
- Motor de reserva com proteção contra concorrência.
- Integração de pagamento e webhooks.

### Semana 7
- Histórico, cancelamento e notificações.
- Dashboard operacional básico.

### Semana 8
- QA final, observabilidade, ajustes de performance e publicação.

## 9) Qualidade e segurança
- Testes automatizados (unitário + integração).
- Logs estruturados, métricas e alertas.
- Rate limit em autenticação.
- Criptografia de dados sensíveis e LGPD.
- Trilha de auditoria para reservas/pagamentos.

## 10) Próximos passos imediatos
1. Escolher stack final (ex.: React Native + NestJS + Postgres).
2. Converter suas telas em backlog técnico (user stories).
3. Definir contratos de API e protótipo navegável integrado.
4. Implementar MVP e validar com 3 a 5 arenas piloto.

---
Se quiser, no próximo passo eu posso transformar este plano em:
- **estrutura de pastas de projeto**,
- **schema SQL inicial**,
- **endpoints com payloads detalhados**,
- **plano de sprints com estimativa por tarefa**.
