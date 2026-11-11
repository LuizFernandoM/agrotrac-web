# AgroTrac Web — Portal Web Administrativo / Operacional

Aplicação web responsável pelo painel de gestão em tempo real do sistema **AgroTrac**, em estrita conformidade com a especificação original (`acac36b6`).

---

## 1. Responsabilidades

- **Painel de Ocupação dos Silos**: Exibição da capacidade total, saldo ocupado e disponibilidade em tempo real para os gerentes do silo.
- **Gestão de Janelas de Agendamento**: Interface para administradores liberarem ou bloquearem janelas de descarga.
- **Acompanhamento da Fila de Recepção**: Visualização dinâmica dos caminhões agendados e fluxo de recebimento de grãos no pátio.
- **Consulta de Pesagens e Auditoria**: Listagem e conferência das pesagens sincronizadas pelo campo e relatórios fiscais.
- **Comunicação em Tempo Real sem Polling**: Conexão ativa via WebSockets (WSS) com o Live B, atualizando dados na tela instantaneamente ao ocorrerem eventos (proibido polling HTTP periódico).
- **Feedback Visual Claro**: Apresentação de estados de carregamento, sucesso, erro e status de conexão com o WebSocket (`conectado`, `reconectando`, `desconectado`).

---

## 2. Ferramentas e Tecnologias

- **Framework / UI**: React com TypeScript.
- **Build Tool**: Vite (rápido e moderno).
- **Estilização**: Tailwind CSS ou CSS Modules responsivo.
- **Cliente WebSocket**: WebSocket nativo ou `socket.io-client` com lógica de reconexão e snapshot.
- **Cliente HTTP**: Axios ou Fetch API.

---

## 3. Arquitetura de Pastas

```text
agrotrac-web/
├── src/
│   ├── components/              # Componentes reutilizáveis (Badges de status, tabelas, modais)
│   ├── pages/                   # Páginas da aplicação
│   │   ├── Dashboard/           # Visão geral de ocupação dos silos
│   │   ├── Windows/             # Administração de janelas de descarga
│   │   ├── Queue/               # Fila de recepção de caminhões
│   │   └── Weighings/           # Consulta de pesagens sincronizadas
│   ├── services/                # Comunicação externa
│   │   ├── api.ts               # Cliente HTTP (baseado na rota /v1 do Gateway)
│   │   └── websocket.ts         # Gerenciamento de conexão WSS e eventos
│   ├── hooks/                   # Hooks customizados (useWebSocket, useOccupancy)
│   ├── types/                   # Tipagens TypeScript dos contratos
│   ├── App.tsx                  # Estrutura de rotas e layout
│   └── main.tsx                 # Ponto de entrada React
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

---

## 4. Conexões e Integrações

- **Comunicação Externa**: Comunica-se exclusivamente com a **API Gateway** (`http://localhost:8080`):
  - HTTP REST: Consome rotas `/v1/core/*`, `/v1/live/*` e `/v1/audit/*`.
  - WebSockets (WSS): Conecta-se em `/live` para receber atualizações instantâneas de reservas e janelas.
