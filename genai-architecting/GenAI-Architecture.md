```mermaid
flowchart TD
    %% Title and Main Actor
    title["Language Learning GenAI Architecture"]
    
    %% Main User
    User([Student/Teacher])
    
    %% Frontend Layer with Specific Activities
    subgraph Frontend["Frontend Layer: Language Learning Portal"]
        direction TB
        UI_Main["Main Interface"]
        UI_SC["Sentence Constructor"]
        UI_VB["Vocabulary Builder"]
        UI_CF["Conversation Practice"]
        UI_Admin["Teacher Admin Panel"]
        
        UI_Main --> UI_SC
        UI_Main --> UI_VB
        UI_Main --> UI_CF
        UI_Main --> UI_Admin
    end
    
    %% Application Layer with Language-Specific Components
    subgraph AppLayer["Application Layer"]
        direction LR
        subgraph Orchestration["Orchestration Services"]
            direction TB
            APIGateway["API Gateway"]
            SessionMgr["Session Manager"]
            ProgressTracker["Learning Progress Tracker"]
        end
        
        subgraph InputProc["Input Processing"]
            direction TB
            InputGuard["Input Guardrails"]
            LangDetect["Language Detection"]
            ContentFilter["Content Filter"]
        end
        
        subgraph ContextEnh["Context Enhancement"]
            direction TB
            KnowledgeRetriever["Knowledge Base Retriever"]
            VectorSearch["Vector Search"]
            ExampleFinder["Example Sentence Finder"]
        end
        
        subgraph OutputProc["Output Processing"]
            direction TB
            ResponseFormat["Response Formatter"]
            OutputGuard["Output Guardrails"]
            LangCorrect["Language Correction"]
        end
    end
    
    %% Model Layer with Specific LLM Implementations
    subgraph ModelLayer["GenAI Models Layer"]
        direction TB
        EmbedModel["Embedding Model<br>(Text → Vectors)"]
        GenModel["Generation Model<br>(Language Production)"]
        TranslateModel["Translation Model<br>(Cross-language)"]
    end
    
    %% Data Layer with Language Learning Data
    subgraph DataLayer["Data Layer"]
        direction LR
        VectorDB["Vector Database<br>(Sentence Examples)"]
        RelationalDB["Relational Database<br>(User Profiles, Progress)"]
        KnowledgeBase["Knowledge Base<br>(Language Rules, Vocabulary)"]
        CacheSys["Cache System<br>(Common Responses)"]
    end
    
    %% Monitoring Layer
    subgraph MonitorLayer["Monitoring Layer"]
        direction TB
        PerfMon["Performance Monitoring"]
        CostTrack["Cost Tracking"]
        EvalMetrics["Evaluation Metrics<br>(Learning Effectiveness)"]
        ErrorLog["Error Logging"]
    end
    
    %% Key Process Flows
    %% Sentence Constructor Flow
    User --> UI_Main
    UI_SC -- "1. Start Sentence\nConstruction" --> APIGateway
    APIGateway -- "2. Process Request" --> InputGuard
    InputGuard -- "3. Validate Input" --> KnowledgeRetriever
    KnowledgeRetriever -- "4. Get Related Content" --> VectorDB
    VectorDB -- "5. Return Vectors" --> VectorSearch
    VectorSearch -- "6. Enhance Query" --> GenModel
    GenModel -- "7. Generate Response" --> OutputGuard
    OutputGuard -- "8. Format Response" --> ResponseFormat
    ResponseFormat -- "9. Return to User" --> UI_SC
    
    %% Data Integration Flows
    KnowledgeRetriever -- "Query Language Rules" --> KnowledgeBase
    VectorSearch -- "Store Vectors" --> VectorDB
    APIGateway -- "Check/Update Progress" --> ProgressTracker
    ProgressTracker -- "Record Activity" --> RelationalDB
    
    %% Model Integration Flows
    InputProc -- "Convert to Vectors" --> EmbedModel
    EmbedModel -- "Store Embeddings" --> VectorDB
    ContextEnh -- "Context for Generation" --> GenModel
    UI_CF -- "Translation Request" --> TranslateModel
    
    %% Monitoring Connections
    GenModel -.- PerfMon
    GenModel -.- CostTrack
    ProgressTracker -.- EvalMetrics
    APIGateway -.- ErrorLog
    
    %% Cache Integration
    GenModel -- "Cache Common Responses" --> CacheSys
    APIGateway -- "Check Cache" --> CacheSys
