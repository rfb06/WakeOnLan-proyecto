# WakeOnLan-proyecto
Power on LAN para maquinas del servidor para el proyecto
graph TD
    A[Inicio] --> B{Validar Argumentos}
    B -->|Inválidos| C[Mostrar Error y Uso]
    C --> Z[Exit 1]
    B -->|--help| D[Mostrar Ayuda]
    D --> Y[Exit 0]
    B -->|Válidos| E[Cargar Configuración]
    
    E --> F{Seleccionar Acción}
    
    F -->|START| G[start_server]
    F -->|STOP| H[stop_server]
    F -->|RESTART| I[restart_server]
    F -->|STATUS| J[status_server]
    
    G --> G1{Verificar Estado Actual}
    G1 -->|Ya está ON| G2[Log: Ya encendido]
    G2 --> Y
    G1 -->|Está OFF| G3[Enviar Magic Packet WoL]
    G3 --> G4[Esperar Arranque<br/>Timeout: 60s]
    G4 --> G5{Servidor Responde?}
    G5 -->|SÍ| G6[Log: Éxito]
    G6 --> Y
    G5 -->|NO| G7[Log: Error Timeout]
    G7 --> Z
    
    H --> H1{Verificar Estado}
    H1 -->|Ya está OFF| H2[Log: Ya apagado]
    H2 --> Y
    H1 -->|Está ON| H3[Ejecutar shutdown via SSH]
    H3 --> H4[Esperar Apagado<br/>30s]
    H4 --> H5{Servidor Apagado?}
    H5 -->|SÍ| H6[Log: Apagado OK]
    H6 --> Y
    H5 -->|NO| H7[Log: Advertencia]
    H7 --> X[Exit 1]
    
    I --> I1[Ejecutar stop_server]
    I1 --> I2[Esperar 10s]
    I2 --> I3[Ejecutar start_server]
    I3 --> Y
    
    J --> J1[Ping Server]
    J1 --> J2{Responde?}
    J2 -->|SÍ| J3[SSH Check]
    J3 --> J4{SSH OK?}
    J4 -->|SÍ| J5[Estado: ONLINE]
    J4 -->|NO| J6[Estado: PARCIAL]
    J2 -->|NO| J7[Estado: OFFLINE]
    J5 --> Y
    J6 --> Y
    J7 --> Y
