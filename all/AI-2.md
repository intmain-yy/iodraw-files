```mermaid
graph TD
    %% 定义 FT 层的样式
    classDef ftNode fill:#F4C7A3,stroke:#A18476,stroke-width:1px;
    
    %% 主层级结构
    subgraph "DISTRIBUTED EDGE-CLOUD PERCEPTION SYSTEM WITH FAULT TOLERANCE (DFD Layer 1)"
        
        %% 边缘集群
        subgraph "EDGE DRONE/VEHICLE CLUSTER (EDGE NODE)"
            
            %% 外部实体和进程
            [Visible Camera] --> |Visible Image Sequence| P1(P1: Visual Frame Queue Extraction)
            [Infrared Detector] --> |Infrared Image Sequence| P2(Process: Infrared Frame Queue Extraction)
            
            %% 连接到联合编码
            P1 --> |Extracted Visual Frames| P3
            P2 --> |Extracted Infrared Frames| P3(Process: Joint Visual/Infrared Shallow DNN Encoding)
            
            %% 注意事项 (作为文本节点插入)
            note1[整合 note1: Joint perception performed here]
            note1 -.-> P3
            
            %% 连接到切分裁决器
            P3 --> |Encoded Feature Representation| P4(Process: System Splitting Point Arbiter)
            
            %% 输出流
            P4 --> |Optimal Splitting Point Data| [D4_label]
            P4 --> |Optimal Split Feature| [D5_label]
        end
        
        %% 网络和容错层
        subgraph "NETWORK & FAULT TOLERANCE (FT) LAYER"
            
            %% 主要传输进程
            P4 --> |"Optimal Split Feature Data (TCP Stream)"| P5(P5: Network Flow Transmission)
            
            %% 容错逻辑
            P5 --> |Continuous High-Frequency UDP Heartbeats| P6(P6: Fault Probe & Backup Node Monitor)
            P6 --> |Primary Failure Trigger| P7(P7: Hot Standby Node Activation & Relay)
            
            %% 应用样式
            class P6,P7 ftNode;
            
            %% 备份路径
            P7 --> |"Reconstructed Data Stream (H_backup)"| P8
            P7 -.-> |"Backup Node Signal"| P6 %% 添加反向监视流
        end
        
        %% 云端节点
        subgraph "CENTER CLOUD SERVER (CLOUD NODE)"
            
            %% 传输流
            P5 --> |TCP Feature Stream| P8
            
            %% 后端处理
            P8(P8: Backend Deep Neural Network Layer) --> |Backend Deep Features| P9(P9: Multimodal High-Dimensional Feature Assembly & Joint Calculation)
            
            %% 最终结果输出
            P9 --> |Final Perception Result| [FinalResult]
            [FinalResult] --> [Output Final Classification/Regression Perception Result]
        end
    end
```