graph TD
    %% User Interaction Layer
    A[User Types in TextareaAutosize] -->|onChange event| B[setInputValue updates state]
    B --> C{User Action}
    C -->|Press Enter| D[handleKeyDown]
    C -->|Click Send Button| E[SendMessageButton onClick]
    
    D --> F[submitMessage called]
    E --> F
    
    %% State Management & Routing
    F --> G{selectedWorkflow?}
    
    %% Workflow Path
    G -->|Yes - Workflow Mode| H[submitWorkflowMessage]
    H --> H1[Create user message]
    H1 --> H2[Add to rawMessages state]
    H2 --> H3[executeWorkflowMutation API call]
    H3 --> H4[POST /backend/workflows/:id/execute]
    H4 --> H5[Backend FastAPI processes]
    H5 --> H6[Response received]
    H6 --> H7[Create bot message]
    H7 --> H8[Add to rawMessages]
    H8 --> H9[Clear inputValue]
    
    %% AI Agent WebSocket Path
    G -->|No - AI Agent Mode| I{Conversation exists?}
    I -->|No| J[createConversationMutation]
    J --> J1[POST /api/conversations]
    J1 --> J2[Node.js saves to PostgreSQL]
    J2 --> K[Set currentConversationId]
    I -->|Yes| K
    
    K --> L[Create userMessage object]
    L --> M[Add to rawMessages state]
    M --> N[Update conversationHistory]
    N --> O[saveMessageMutation]
    O --> O1[POST /api/messages]
    O1 --> O2[Node.js saves to PostgreSQL]
    
    N --> P[wsSendMessage called]
    P --> Q[useWebSocket.sendMessage]
    Q --> R[JSON.stringify payload]
    R --> S[WebSocket.send]
    
    %% WebSocket Connection
    S -->|WebSocket| T[Agent FastAPI /ws endpoint]
    T --> U[WebSocket.receive_json]
    U --> V[Validate WebsocketRequest model]
    V --> V1{Valid?}
    V1 -->|No| V2[Send error response]
    V1 -->|Yes| W[Extract: query, history, region, generate_title]
    
    %% Agent Processing
    W --> X{generate_title?}
    X -->|Yes| Y[_create_title_for_conversation]
    Y --> Z[LLM generates title]
    Z --> AA[send_ws_message type: 'title']
    AA -->|WebSocket| AB[Frontend receives title]
    AB --> AC[Update conversation title]
    
    X -->|No/After| AD[Get region config]
    AD --> AE[Initialize multi-agent system]
    AE --> AF[Agent workflow executes]
    AF --> AG[Stream responses]
    AG --> AH[send_ws_message multiple times]
    AH -->|WebSocket| AI[Frontend handleWebSocketMessage]
    
    %% Response Processing
    AI --> AJ{Message type?}
    AJ -->|'stream'| AK[Append to latest bot message]
    AJ -->|'chart'| AL[Add chart data to message]
    AJ -->|'status'| AM[Update status panel]
    AJ -->|'title'| AN[Set conversation title]
    AJ -->|'complete'| AO[Set isLoading = false]
    
    AK --> AP[Update rawMessages state]
    AL --> AP
    AM --> AP
    AN --> AP
    
    AP --> AQ[React re-renders UI]
    AQ --> AR[User sees response]
    
    %% Clear Input
    O2 --> AS[Clear files array]
    AS --> AT[setInputValue empty string]
    AT --> AU[Input cleared - ready for next message]
    
    %% Stop Generation Path
    AV[User clicks Stop] --> AW[stop function]
    AW --> AX[wsSendMessage type: 'stop_generation']
    AX -->|WebSocket| AY[Agent receives stop]
    AY --> AZ[Cancel agent_task]
    AZ --> BA[Set isLoading = false]
    
    style A fill:#e1f5ff
    style F fill:#fff4e6
    style P fill:#ffe7e7
    style T fill:#f3e8ff
    style AI fill:#e7f5e7
    style AQ fill:#fff9e6