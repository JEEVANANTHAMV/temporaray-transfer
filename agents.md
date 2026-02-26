# Hot Takes AI Integration - Complete Architecture Diagrams

## Overview

This document contains comprehensive Mermaid diagrams for the AI-powered survey creation pipeline. Each agent is detailed with its tools, autonomy levels, response actions, and data flow.

---

## 1. Main Architecture Overview

```mermaid
flowchart TB
    subgraph Brand["👤 BRAND LAYER"]
        BD[Brand Dashboard]
        BInput[User Input]
        BApprove[Approval Actions]
    end

    subgraph Admin["🔧 ADMIN LAYER"]
        AD[Admin Dashboard]
        AIntervene[Intervention Panel]
        AReview[Review & Override]
        AAudit[Audit Logs]
    end

    subgraph Pipeline["🤖 AI AGENT PIPELINE"]
        direction LR
        subgraph A1["Agent 1: Intelligent Intake"]
            IIA[Intake Agent]
            ITools[Tools: Chat UI<br/>Knowledge Base<br/>NLU Engine]
        end
        
        subgraph A2["Agent 2: Research Plan"]
            RPG[Plan Generator]
            RTools[Tools: Hypothesis Engine<br/>Methodology DB<br/>Reasoning Module]
        end
        
        subgraph A3["Agent 3: Questionnaire"]
            QGM[Questionnaire & Game Mapper]
            QTools[Tools: Question DB<br/>Template Matcher<br/>Coverage Checker]
        end
        
        subgraph A4["Agent 4: Auto-Launch"]
            AUL[Auto-Upload & Launch]
            LTools[Tools: Supabase Writer<br/>Validator<br/>Launch Controller]
        end
    end

    subgraph LLM["🧠 MODEL-AGNOSTIC LLM LAYER"]
        ML[Model Abstraction Layer]
        GPT[OpenAI GPT]
        CLAUDE[Anthropic Claude]
        GEMINI[Google Gemini]
        LOCAL[Local Models]
    end

    subgraph Knowledge["📚 KNOWLEDGE SOURCES"]
        KB[(Knowledge Base)]
        TMPL[Game Templates<br/>10-12 Templates]
        SCHEMA[(Supabase Schema)]
        REFS[Research Frameworks]
    end

    subgraph Data["💾 DATA LAYER"]
        SUPA[(Supabase<br/>PostgreSQL)]
        STORAGE[Document Storage<br/>Versions & History]
    end

    subgraph Autonomy["📊 AUTONOMY LEVELS"]
        L1["Level 1: Co-Pilot<br/>AI drafts, Human approves"]
        L2["Level 2: Supervised Autopilot<br/>AI executes, Flags edge cases"]
        L3["Level 3: Full Autopilot<br/>AI runs end-to-end"]
    end

    %% Connections
    BD --> BInput --> IIA
    AD --> AIntervene -.-> IIA
    AReview -.-> RPG
    AReview -.-> QGM
    AReview -.-> AUL

    IIA --> |Intake Brief| RPG
    RPG --> |Research Plan| QGM
    QGM --> |Questionnaire| AUL
    AUL --> |Launch Data| SUPA

    IIA -.-> ITools
    RPG -.-> RTools
    QGM -.-> QTools
    AUL -.-> LTools

    ITools --> ML
    RTools --> ML
    QTools --> ML
    LTools --> ML

    ML --> GPT & CLAUDE & GEMINI & LOCAL

    KB & TMPL & SCHEMA & REFS --> Knowledge
    Knowledge --> IIA & RPG & QGM

    SUPA --> STORAGE
    AUL --> AAudit

    style A1 fill:#E3F2FD,stroke:#1976D2
    style A2 fill:#E8F5E9,stroke:#388E3C
    style A3 fill:#FFF3E0,stroke:#F57C00
    style A4 fill:#FCE4EC,stroke:#C2185B
    style LLM fill:#F3E5F5,stroke:#7B1FA2
    style Autonomy fill:#ECEFF1,stroke:#455A64
```

---

## 2. Agent 1: Intelligent Intake Agent - Detailed Flow

```mermaid
flowchart TB
    subgraph Input["📥 INPUT"]
        START([Brand Opens Dashboard])
        BINFO[Brand Profile<br/>Industry, History]
    end

    subgraph Agent["🤖 INTELLIGENT INTAKE AGENT"]
        direction TB
        
        subgraph Tools["🛠️ TOOLS"]
            CHAT[Chat Interface<br/>WebSocket Real-time]
            NLU[NLU Engine<br/>Intent Detection]
            NER[Entity Extraction<br/>Products, Segments, KPIs]
            KB[Knowledge Retrieval<br/>Industry Patterns]
            CONTEXT[Context Manager<br/>Session State]
        end

        subgraph Flow["📊 DISCOVERY FLOW"]
            GREET[1. Greeting &<br/>Context Setting]
            BUSINESS[2. Business Goals<br/>& Brand Context]
            AUDIENCE[3. Target Audience<br/>& Segmentation]
            OBJECTIVES[4. Research Objectives<br/>& Hypotheses]
            MECHANICS[5. Survey Mechanics<br/>& Game Templates]
            HANDLING[6. Data Handling<br/>& Reporting]
            CONFIRM[7. Summary &<br/>Confirmation]
        end

        subgraph Output["📤 OUTPUT"]
            TRANSCRIPT[Full Transcript<br/>JSON + Text]
            BRIEF[Intake Brief<br/>Structured JSON]
            ENTITIES[Extracted Entities<br/>Products, Segments, KPIs]
        end
    end

    subgraph Autonomy["🎯 AUTONOMY LEVELS"]
        direction LR
        L1A["L1: Co-Pilot<br/>AI asks questions<br/>Brand confirms brief"]
        L2A["L2: Supervised<br/>AI flags ambiguous<br/>responses"]
        L3A["L3: Full Auto<br/>AI handles standard<br/>intakes autonomously"]
    end

    subgraph Actions["⚡ RESPONSE ACTIONS"]
        ASK[Ask Clarifying<br/>Questions]
        VALIDATE[Validate<br/>Responses]
        FLAG[Flag Edge Cases<br/>& Ambiguities]
        REVISE[Revise Brief<br/>on Feedback]
        STORE[Store Documents<br/>& Session]
    end

    subgraph Admin["🔧 ADMIN INTERVENTION"]
        VIEW[View Live<br/>Transcript]
        INTERVENE[Intervene &<br/>Correct Course]
        OVERRIDE[Override AI<br/>Responses]
    end

    subgraph Decision["🔀 AUTONOMY DECISION"]
        CHECK{Edge Case<br/>Detected?}
        COMPLEX{Complex<br/>Requirements?}
        ESCALATE[Elevate to<br/>Higher Human Role]
        PROCEED[Proceed to<br/>Agent 2]
    end

    %% Flow Connections
    START --> GREET
    BINFO --> CONTEXT
    
    GREET --> BUSINESS --> AUDIENCE --> OBJECTIVES --> MECHANICS --> HANDLING --> CONFIRM
    
    CHAT & NLU & NER & KB & CONTEXT --> GREET & BUSINESS & AUDIENCE & OBJECTIVES & MECHANICS & HANDLING
    
    CONFIRM --> TRANSCRIPT & BRIEF & ENTITIES
    
    BRIEF --> CHECK
    CHECK -->|Yes| FLAG --> ESCALATE
    CHECK -->|No| COMPLEX
    COMPLEX -->|Yes| INTERVENE --> REVISE
    COMPLEX -->|No| PROCEED
    
    VIEW --> TRANSCRIPT
    INTERVENE -.-> GREET & BUSINESS & AUDIENCE
    OVERRIDE -.-> BRIEF

    ASK & VALIDATE & FLAG & REVISE & STORE --> CHECK

    L1A -.-> ASK
    L2A -.-> FLAG
    L3A -.-> PROCEED

    style Agent fill:#E3F2FD,stroke:#1976D2
    style Tools fill:#BBDEFB,stroke:#1976D2
    style Flow fill:#90CAF9,stroke:#1976D2
    style Autonomy fill:#ECEFF1,stroke:#455A64
```

---

## 3. Agent 2: Research Plan Generator - Detailed Flow

```mermaid
flowchart TB
    subgraph Input["📥 INPUT"]
        BRIEF[Intake Brief<br/>From Agent 1]
        ENTITIES[Extracted Entities]
        HISTORY[Brand History<br/>Previous Studies]
    end

    subgraph Agent["🤖 RESEARCH PLAN GENERATOR"]
        direction TB
        
        subgraph Tools["🛠️ TOOLS"]
            HYP[Hypothesis Engine<br/>Pattern Matching]
            METHOD[Methodology DB<br/>Best Practices]
            REASON[Reasoning Module<br/>Explainability]
            CONTEXT[Context Flow<br/>Intake Integration]
            VERSION[Version Control<br/>Plan History]
        end

        subgraph Flow["📊 GENERATION FLOW"]
            ANALYZE[1. Analyze Intake<br/>& Extract Objectives]
            QUESTIONS[2. Generate Primary<br/>Research Questions]
            HYPGEN[3. Generate Hypotheses<br/>& Sub-Hypotheses]
            EXPLAIN[4. Add Reasoning<br/>For Each Hypothesis]
            METHODOLOGY[5. Define Methodology<br/>& Approach]
            TEMPLATES[6. Recommend Game<br/>Templates]
            REVIEW[7. Consistency Check<br/>& Validation]
        end

        subgraph Output["📤 OUTPUT"]
            PLAN[Research Plan<br/>Structured JSON]
            HYPDOC[Hypothesis Document<br/>with Reasoning]
            TEMPLATE[Template<br/>Recommendations]
            VERSIONDOC[Version History<br/>Changelog]
        end
    end

    subgraph Autonomy["🎯 AUTONOMY LEVELS"]
        direction LR
        L1A["L1: Co-Pilot<br/>AI generates plan<br/>Brand approves/edits"]
        L2A["L2: Supervised<br/>AI flags weak<br/>hypotheses"]
        L3A["L3: Full Auto<br/>AI creates plans<br/>for standard studies"]
    end

    subgraph Actions["⚡ RESPONSE ACTIONS"]
        GENHYP[Generate<br/>Hypotheses]
        REASONING[Provide<br/>Reasoning]
        CONSISTENCY[Check Logical<br/>Consistency]
        ITERATE[Iterate on<br/>Feedback]
        VERSIONING[Track<br/>Versions]
    end

    subgraph Consistency["🔄 LOGICAL CONSISTENCY ENGINE"]
        direction TB
        VALIDATE[Validate Against<br/>Intake Brief]
        CROSSREF[Cross-Reference<br/>Hypotheses]
        GAP[Identify Coverage<br/>Gaps]
        ALIGN[Align with<br/>Objectives]
    end

    subgraph Feedback["💬 FEEDBACK LOOP"]
        PREVIEW[Brand Preview<br/>Plan]
        FEEDBACK[Brand Provides<br/>Feedback]
        REVISE[AI Revises<br/>Plan]
        MAINTAIN[Maintain Logical<br/>Consistency]
    end

    subgraph Decision["🔀 AUTONOMY DECISION"]
        CHECK{Hypothesis<br/>Quality OK?}
        COVERAGE{All Objectives<br/>Covered?}
        COMPLEX{Complex<br/>Study?}
        ESCALATE[Elevate to<br/>Admin Review]
        PROCEED[Proceed to<br/>Agent 3]
    end

    %% Flow Connections
    BRIEF & ENTITIES & HISTORY --> ANALYZE
    
    ANALYZE --> QUESTIONS --> HYPGEN --> EXPLAIN --> METHODOLOGY --> TEMPLATES --> REVIEW
    
    HYP & METHOD & REASON & CONTEXT & VERSION --> ANALYZE & QUESTIONS & HYPGEN & EXPLAIN
    
    REVIEW --> PLAN & HYPDOC & TEMPLATE & VERSIONDOC
    
    CONSISTENCY --> VALIDATE --> CROSSREF --> GAP --> ALIGN
    ALIGN --> REVIEW
    
    PLAN --> PREVIEW --> FEEDBACK --> REVISE --> MAINTAIN
    MAINTAIN --> PLAN
    
    PLAN --> CHECK
    CHECK -->|No| ESCALATE
    CHECK -->|Yes| COVERAGE
    COVERAGE -->|No| GAP
    COVERAGE -->|Yes| COMPLEX
    COMPLEX -->|Yes| ESCALATE
    COMPLEX -->|No| PROCEED

    GENHYP & REASONING & CONSISTENCY & ITERATE & VERSIONING --> REVIEW

    style Agent fill:#E8F5E9,stroke:#388E3C
    style Tools fill:#C8E6C9,stroke:#388E3C
    style Flow fill:#A5D6A7,stroke:#388E3C
    style Consistency fill:#DCEDC8,stroke:#689F38
```

---

## 4. Agent 3: Questionnaire Generator & Game Mapper - Detailed Flow

```mermaid
flowchart TB
    subgraph Input["📥 INPUT"]
        PLAN[Research Plan<br/>From Agent 2]
        HYPDOC[Hypothesis Document]
        TEMPLATES[Template<br/>Recommendations]
    end

    subgraph Agent["🤖 QUESTIONNAIRE GENERATOR & GAME MAPPER"]
        direction TB
        
        subgraph Tools["🛠️ TOOLS"]
            QDB[Question Bank<br/>Validated Questions]
            TMPL[Template Matcher<br/>10-12 Game Templates]
            COVERAGE[Hypothesis Coverage<br/>Checker]
            FORMAT[Format Selector<br/>Question Types]
            PREVIEW[Preview Engine<br/>Brand View]
        end

        subgraph Templates["🎮 GAME TEMPLATES (10-12)"]
            T1[Template 1:<br/>Rating Scale]
            T2[Template 2:<br/>Multiple Choice]
            T3[Template 3:<br/>Ranking]
            T4[Template 4:<br/>Open-Ended]
            T5[Template 5:<br/>Matrix]
            T6[Template 6-12:<br/>Custom Games]
        end

        subgraph Flow["📊 GENERATION FLOW"]
            PARSE[1. Parse Research Plan<br/>& Hypotheses]
            QGEN[2. Generate Questions<br/>Per Hypothesis]
            FSELECT[3. Select Question<br/>Formats]
            TMAP[4. Map to Game<br/>Templates]
            COVCHECK[5. Run Coverage<br/>Check]
            OPTIMIZE[6. Optimize Flow<br/>& Length]
            VALIDATE[7. Final Validation<br/>& Quality]
        end

        subgraph Output["📤 OUTPUT"]
            QUEST[Questionnaire<br/>Structured JSON]
            MAPPING[Question-Template<br/>Mapping]
            COVDOC[Coverage Report<br/>Per Hypothesis]
            PREVIEWD[Preview Data<br/>For Brand]
        end
    end

    subgraph Autonomy["🎯 AUTONOMY LEVELS"]
        direction LR
        L1A["L1: Co-Pilot<br/>AI generates questionnaire<br/>Brand approves each question"]
        L2A["L2: Supervised<br/>AI flags low-coverage<br/>areas"]
        L3A["L3: Full Auto<br/>AI creates questionnaires<br/>for standard templates"]
    end

    subgraph Actions["⚡ RESPONSE ACTIONS"]
        GENQ[Generate<br/>Questions]
        MATCH[Match<br/>Templates]
        CHECKCOV[Check<br/>Coverage]
        OPTIMIZEQ[Optimize<br/>Flow]
        VALIDATEQ[Validate<br/>Quality]
    end

    subgraph Coverage["📈 HYPOTHESIS COVERAGE CHECKER"]
        direction TB
        MAPHYP[Map Questions<br/>to Hypotheses]
        CALC[Calculate Coverage<br/>% per Hypothesis]
        IDENTIFY[Identify<br/>Uncovered Areas]
        SUGGEST[Suggest<br/>Additional Questions]
        REPORT[Generate<br/>Coverage Report]
    end

    subgraph Feedback["💬 FEEDBACK LOOP"]
        PREVIEWQ[Brand Preview<br/>Questionnaire]
        EDIT[Brand Edits<br/>Questions]
        REGEN[AI Regenerates<br/>Affected Parts]
        REMAP[Remap to<br/>Templates]
        RECHECK[Re-run<br/>Coverage Check]
    end

    subgraph Decision["🔀 AUTONOMY DECISION"]
        CHECK{Coverage ≥ 90%?}
        TEMPLATE{Template<br/>Match OK?}
        QUALITY{Question<br/>Quality OK?}
        ESCALATE[Elevate to<br/>Admin Review]
        PROCEED[Proceed to<br/>Agent 4]
    end

    %% Flow Connections
    PLAN & HYPDOC & TEMPLATES --> PARSE
    
    PARSE --> QGEN --> FSELECT --> TMAP --> COVCHECK --> OPTIMIZE --> VALIDATE
    
    QDB & TMPL & COVERAGE & FORMAT & PREVIEW --> QGEN & FSELECT & TMAP & COVCHECK
    
    T1 & T2 & T3 & T4 & T5 & T6 --> TMPL
    
    VALIDATE --> QUEST & MAPPING & COVDOC & PREVIEWD
    
    COVCHECK --> MAPHYP --> CALC --> IDENTIFY --> SUGGEST --> REPORT
    
    QUEST --> PREVIEWQ --> EDIT --> REGEN --> REMAP --> RECHECK
    RECHECK --> COVCHECK
    
    QUEST --> CHECK
    CHECK -->|No| IDENTIFY
    CHECK -->|Yes| TEMPLATE
    TEMPLATE -->|No| ESCALATE
    TEMPLATE -->|Yes| QUALITY
    QUALITY -->|No| ESCALATE
    QUALITY -->|Yes| PROCEED

    GENQ & MATCH & CHECKCOV & OPTIMIZEQ & VALIDATEQ --> VALIDATE

    style Agent fill:#FFF3E0,stroke:#F57C00
    style Tools fill:#FFE0B2,stroke:#F57C00
    style Templates fill:#FFCC80,stroke:#EF6C00
    style Coverage fill:#FFE0B2,stroke:#F57C00
```

---

## 5. Agent 4: Auto-Upload & Launch - Detailed Flow

```mermaid
flowchart TB
    subgraph Input["📥 INPUT"]
        QUEST[Questionnaire<br/>From Agent 3]
        MAPPING[Question-Template<br/>Mapping]
        SETTINGS[Brand Settings<br/>Sample, Duration]
    end

    subgraph Agent["🤖 AUTO-UPLOAD & LAUNCH"]
        direction TB
        
        subgraph Tools["🛠️ TOOLS"]
            SUPAW[Supabase Writer<br/>DB Operations]
            VALIDATOR[Pre-launch<br/>Validator]
            LAUNCHC[Launch Controller<br/>Workflow Engine]
            MONITOR[Monitoring<br/>System]
            NOTIFIER[Notification<br/>Service]
        end

        subgraph Validation["✅ PRE-LAUNCH VALIDATIONS"]
            direction TB
            V1[1. Schema Validation<br/>Correct Data Types]
            V2[2. Template Validation<br/>Game Instance Compatibility]
            V3[3. Coverage Validation<br/>All Hypotheses Covered]
            V4[4. Logic Validation<br/>Skip Logic, Branching]
            V5[5. Quality Validation<br/>No Duplicates, Errors]
            V6[6. Settings Validation<br/>Sample Size, Duration]
        end

        subgraph Flow["📊 LAUNCH FLOW"]
            PREP[1. Prepare Data<br/>for Upload]
            VAL[2. Run Pre-launch<br/>Validations]
            UPLOAD[3. Upload to<br/>Supabase]
            ATTACH[4. Attach Game<br/>Instances]
            CONFIG[5. Configure Settings<br/>Sample, Duration]
            SCHEDULE[6. Schedule or<br/>Immediate Launch]
            GO[LIVE! 🚀]
        end

        subgraph Output["📤 OUTPUT"]
            SURVEYID[Survey ID<br/>& Access URL]
            LOGS[Deployment Logs<br/>Timestamped]
            REPORT[Validation Report<br/>Passed/Failed]
            DASHBOARD[Live Dashboard<br/>Access]
        end
    end

    subgraph Autonomy["🎯 AUTONOMY LEVELS"]
        direction LR
        L1A["L1: Co-Pilot<br/>AI prepares everything<br/>Brand clicks Launch"]
        L2A["L2: Supervised<br/>AI launches, flags<br/>validation warnings"]
        L3A["L3: Full Auto<br/>AI launches & notifies<br/>brand when live"]
    end

    subgraph Actions["⚡ RESPONSE ACTIONS"]
        PREPARE[Prepare<br/>Upload Data]
        VALIDATE[Run<br/>Validations]
        WRITE[Write to<br/>Database]
        ATTACHG[Attach<br/>Games]
        LAUNCHA[Execute<br/>Launch]
        NOTIFY[Notify<br/>Stakeholders]
    end

    subgraph Database["💾 SUPABASE SCHEMA"]
        direction TB
        T1[(surveys)]
        T2[(questions)]
        T3[(game_instances)]
        T4[(responses)]
        T5[(settings)]
    end

    subgraph Error["⚠️ ERROR HANDLING"]
        RETRY[Retry Failed<br/>Operations]
        ROLLBACK[Rollback on<br/>Critical Failure]
        LOGERR[Log Errors<br/>for Debug]
        ALERT[Alert Admin<br/>Team]
    end

    subgraph Decision["🔀 AUTONOMY DECISION"]
        CHECK{All Validations<br/>Passed?}
        WARNINGS{Warnings<br/>Present?}
        CRITICAL{Critical<br/>Errors?}
        FIX[Fix Issues<br/>& Retry]
        APPROVE[Admin Approval<br/>for Warnings]
        PROCEED[Launch<br/>Survey]
    end

    %% Flow Connections
    QUEST & MAPPING & SETTINGS --> PREP
    
    PREP --> VAL --> UPLOAD --> ATTACH --> CONFIG --> SCHEDULE --> GO
    
    SUPAW & VALIDATOR & LAUNCHC & MONITOR & NOTIFIER --> PREP & VAL & UPLOAD & ATTACH & CONFIG & SCHEDULE
    
    V1 --> V2 --> V3 --> V4 --> V5 --> V6
    VAL --> V1
    
    UPLOAD --> T1 & T2 & T3 & T5
    ATTACH --> T3
    GO --> T4
    
    GO --> SURVEYID & LOGS & REPORT & DASHBOARD
    
    VAL --> CHECK
    CHECK -->|No| CRITICAL
    CRITICAL -->|Yes| ROLLBACK --> LOGERR --> ALERT --> FIX
    CRITICAL -->|No| WARNINGS
    WARNINGS -->|Yes| APPROVE --> PROCEED
    WARNINGS -->|No| PROCEED
    CHECK -->|Yes| PROCEED
    
    RETRY --> UPLOAD

    PREPARE & VALIDATE & WRITE & ATTACHG & LAUNCHA & NOTIFY --> GO

    style Agent fill:#FCE4EC,stroke:#C2185B
    style Tools fill:#F8BBD9,stroke:#C2185B
    style Validation fill:#FCE4EC,stroke:#C2185B
    style Database fill:#F3E5F5,stroke:#7B1FA2
    style Error fill:#FFEBEE,stroke:#C62828
```

---

## 6. End-to-End Autonomy Progression

```mermaid
flowchart LR
    subgraph Phase1["📊 PHASE 1: AI-POWERED SURVEY CREATION"]
        direction LR
        
        subgraph A1Flow["Agent 1: Intake"]
            direction TB
            A1L1["L1: Co-Pilot<br/>📝 AI Asks → ✋ Human Confirms"]
            A1L2["L2: Supervised<br/>🤖 AI Executes → ⚠️ Flags Ambiguity"]
            A1L3["L3: Full Auto<br/>🚀 AI Runs Solo → 📋 Human Monitors"]
        end
        
        subgraph A2Flow["Agent 2: Research Plan"]
            direction TB
            A2L1["L1: Co-Pilot<br/>📝 AI Generates → ✋ Human Approves"]
            A2L2["L2: Supervised<br/>🤖 AI Creates → ⚠️ Flags Weak Hypotheses"]
            A2L3["L3: Full Auto<br/>🚀 AI Runs Solo → 📋 Maintains Consistency"]
        end
        
        subgraph A3Flow["Agent 3: Questionnaire"]
            direction TB
            A3L1["L1: Co-Pilot<br/>📝 AI Creates → ✋ Human Reviews Each"]
            A3L2["L2: Supervised<br/>🤖 AI Builds → ⚠️ Flags Low Coverage"]
            A3L3["L3: Full Auto<br/>🚀 AI Generates → 📋 Quality Checks"]
        end
        
        subgraph A4Flow["Agent 4: Launch"]
            direction TB
            A4L1["L1: Co-Pilot<br/>📝 AI Prepares → ✋ Human Clicks Launch"]
            A4L2["L2: Supervised<br/>🤖 AI Launches → ⚠️ Flags Warnings"]
            A4L3["L3: Full Auto<br/>🚀 AI Launches → 📋 Notifies When Live"]
        end
    end

    A1Flow -->|Intake Brief| A2Flow
    A2Flow -->|Research Plan| A3Flow
    A3Flow -->|Questionnaire| A4Flow

    style A1Flow fill:#E3F2FD,stroke:#1976D2
    style A2Flow fill:#E8F5E9,stroke:#388E3C
    style A3Flow fill:#FFF3E0,stroke:#F57C00
    style A4Flow fill:#FCE4EC,stroke:#C2185B
```

---

## 7. Complete Data Flow with Autonomy Indicators

```mermaid
flowchart TB
    subgraph L1["LEVEL 1: CO-PILOT (Current Phase 1)"]
        direction TB
        
        B1[👤 Brand Opens Dashboard]
        
        subgraph A1L1["Agent 1: Intelligent Intake"]
            A1Q[AI Asks Questions]
            A1D[AI Drafts Brief]
            H1A[✋ Human Reviews & Approves]
            H1B[📄 Intake Brief<br/>+ Transcript]
        end
        
        subgraph A2L1["Agent 2: Research Plan"]
            A2G[AI Generates Plan]
            A2H[AI Adds Reasoning]
            H2A[✋ Human Reviews & Edits]
            H2B[📄 Research Plan<br/>+ Hypotheses]
        end
        
        subgraph A3L1["Agent 3: Questionnaire"]
            A3G[AI Generates Questions]
            A3M[AI Maps Templates]
            A3C[AI Checks Coverage]
            H3A[✋ Human Reviews Each Q]
            H3B[📄 Questionnaire<br/>+ Mappings]
        end
        
        subgraph A4L1["Agent 4: Auto-Launch"]
            A4V[AI Validates]
            A4P[AI Prepares Upload]
            H4A[✋ Human Clicks Launch]
            H4B[🚀 Survey Live]
        end
        
        B1 --> A1L1
        A1Q --> A1D --> H1A --> H1B
        H1B --> A2L1
        A2G --> A2H --> H2A --> H2B
        H2B --> A3L1
        A3G --> A3M --> A3C --> H3A --> H3B
        H3B --> A4L1
        A4V --> A4P --> H4A --> H4B
    end

    style A1L1 fill:#E3F2FD,stroke:#1976D2
    style A2L1 fill:#E8F5E9,stroke:#388E3C
    style A3L1 fill:#FFF3E0,stroke:#F57C00
    style A4L1 fill:#FCE4EC,stroke:#C2185B
```

---

## 8. Summary: Tools Per Agent

| Agent | Primary Tools | Knowledge Sources | Output | Autonomy Level |
|-------|--------------|-------------------|--------|----------------|
| **1. Intelligent Intake** | Chat UI, NLU Engine, Entity Extraction, Context Manager | Brand Profile, Industry Patterns | Intake Brief, Transcript, Entities | L1: AI drafts → Human approves |
| **2. Research Plan Generator** | Hypothesis Engine, Methodology DB, Reasoning Module, Version Control | Research Frameworks, Methodologies | Research Plan, Hypotheses, Reasoning | L1: AI generates → Human edits |
| **3. Questionnaire Generator** | Question Bank, Template Matcher, Coverage Checker, Format Selector | Game Templates (10-12), Question Types | Questionnaire, Mappings, Coverage Report | L1: AI creates → Human reviews |
| **4. Auto-Upload & Launch** | Supabase Writer, Validator, Launch Controller, Monitor | Supabase Schema, Validation Rules | Survey ID, Logs, Dashboard Access | L1: AI prepares → Human launches |

---

*Document generated for Hot Takes AI Integration Project*
