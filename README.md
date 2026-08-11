%%{init: {'flowchart': {'curve': 'basis', 'nodeSpacing': 15, 'rankSpacing': 25}}}%%
flowchart LR
    %% Sensor Inputs
    ENC_L["Left Encoder"]
    ENC_R["Right Encoder"]
    IMU["IMU (Gyro)"]
    WALL["IR Wall Sensors"]

    %% State Estimation
    FWD["Forward Estimate"]
    ROT["Rotation Estimate"]
    STEER["Steering Correction"]

    %% Motion Planning & Modeling
    MP["Motion Profiler"]
    MIX["Wheel Target Mixing"]
    TARGETS["Wheel Targets<br/>(Left / Right)"]
    SYSID["MATLAB SysID Models<br/>(Left / Right Wheels)"]

    %% Control System
    DIST["Disturbance Model<br/><i>(Friction & IR)</i>"]
    FF["Feedforward"]
    FB["Feedback"]
    SUM(("+"))
    MOTORS["Motors<br/>(Left / Right)"]

    %% Sensing & Estimation Connections
    ENC_L & ENC_R --> FWD
    ENC_L & ENC_R & IMU --> ROT
    WALL --> STEER

    %% Planning Connections
    MP & FWD & ROT --> MIX --> TARGETS
    TARGETS --> SYSID --> FF
    TARGETS & STEER --> FB

    %% Disturbance (WIP)
    WALL -.-> DIST -.-> FF

    %% Control & Actuation
    FF & FB --> SUM --> MOTORS

    %% Styling
    classDef model fill:#f5eefc,stroke:#7a3fa0,stroke-width:1.5px,color:#1a1a1a;
    classDef sensor fill:#fff4e6,stroke:#c97a1a,stroke-width:1.5px,color:#1a1a1a;
    classDef estimate fill:#eaf1fb,stroke:#2b4c7e,stroke-width:1.5px,color:#1a1a1a;
    classDef controller fill:#e8f8ee,stroke:#1f7a4d,stroke-width:1.5px,color:#1a1a1a;
    classDef motor fill:#fdeaea,stroke:#b23b3b,stroke-width:1.5px,color:#1a1a1a;
    classDef wip fill:#f2f2f2,stroke:#999999,stroke-width:1.5px,stroke-dasharray:4 4,color:#555555;

    class SYSID,MP model;
    class ENC_L,ENC_R,IMU,WALL sensor;
    class FWD,ROT,STEER,MIX,TARGETS estimate;
    class FF,FB,SUM controller;
    class MOTORS motor;
    class DIST wip;
