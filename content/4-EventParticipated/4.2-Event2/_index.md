---
title: "Event 2"
date: "2025-11-15"
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: “AWS Cloud Mastery Series #1 – AI/ML/GenAI on AWS”

### Event Objectives

- Provide an overview of key AI/ML/GenAI capabilities available on AWS.
- Highlight practical building blocks such as Amazon Bedrock, pretrained AI services, and AgentCore for real-world solutions.

### Speakers

- **Lam Tuan Kiet** – Sr DevOps Engineer, FPT Software  
- **Danh Hoang Hieu Nghi** – AI Engineer, Renova Cloud  
- **Dinh Le Hoang Anh** – Cloud Engineer Trainee, First Cloud AI Journey  
- **Van Hoang Kha** – Cloud Security Engineer, AWS Community Builder  

### Key Highlights

#### Generative AI with Amazon Bedrock

**- Foundation Models:**  
Bedrock exposes multiple foundation models (FMs) through a managed API, allowing teams to plug in models from different providers and adapt them to tasks like chat, search, and summarization without managing underlying infrastructure.

**- Prompt Engineering:**  
Covered practical prompting patterns for controlling model behavior:

- Zero-shot prompting: Ask the model to solve a task directly with only the instruction.  
- Few-shot prompting: Add a few examples to guide the model toward the desired format and style.  
- Chain-of-thought: Ask the model to reason step by step so it can expose intermediate logic before giving a final answer.  

**- Retrieval-Augmented Generation (RAG):**  
RAG pipelines combine search and generation so the model answers questions using your own data, not just its pretraining.

- R – Retrieval: Fetch relevant chunks from a knowledge base or vector store.  
- A – Augmented: Attach retrieved content to the prompt as extra context.  
- G – Generation: Let the model generate an answer grounded in that context.  
- Common use cases: domain-aware chatbots, question answering over internal docs, semantic search, and near real-time content summarization.

**- Amazon Titan Embeddings:**  
Titan Embeddings converts text into numeric vectors that capture semantic meaning, enabling more accurate similarity search, RAG, recommendations, and clustering across 100+ languages.

**- Pretrained AI Services on AWS:**  
The session briefly walked through managed AI APIs that hide ML complexity behind simple calls:

- Amazon Rekognition – Image and video analysis (labeling, faces, moderation, etc.).  
- Amazon Translate – Automatic language detection and translation.  
- Amazon Textract – Extracts text and layout from documents.  
- Amazon Transcribe – Converts speech to text for captions and call analytics.  
- Amazon Polly – Text-to-speech for natural-sounding voices.  
- Amazon Comprehend – NLP for entities, sentiment, and key phrase extraction.  
- Amazon Kendra – Enterprise search over heterogeneous content sources.  
- Amazon Lookout services – Detect anomalies in metrics, equipment data, and images.  
- Amazon Personalize – Recommendation engine for personalized rankings and suggestions.

**- Demo – AMZPhoto:**  
Demonstration app that uses Bedrock and image recognition to identify faces from uploaded photos and associate them with stored metadata, illustrating a typical end-to-end pipeline from ingestion to inference.

#### Amazon Bedrock AgentCore

**- Overview:**  
AgentCore is a modular, fully managed platform for building and operating AI agents in production, handling runtime, security, and observability concerns so teams can focus on agent logic.

**- What AgentCore Solves:**  

- Securely execute and scale agent code with a dedicated runtime.  
- Provide short-term and long-term memory so agents can learn from prior interactions.  
- Integrate with identity systems to enforce access control for agents and tools.  
- Connect agents to tools and APIs (via Gateway and MCP) for complex workflows.  
- Offer observability and evaluations to monitor quality and trace agent decisions.  

**- Core Service Groups (as presented):**

- **Foundational services:** Runtime, Identity, Gateway, Policy – handle secure execution, tool access, and guardrails.  
- **Tools & memory layer:** Memory, Browser tool, Code Interpreter, and other integrations used to extend agent capabilities.  
- **Secure deployment at scale:** Serverless Runtime and Identity support multi-tenant and enterprise use cases.  
- **Operational insights:** Observability and Evaluations provide traces, metrics, and continuous quality checks.

**- Frameworks for Building Agents:**  
AgentCore interoperates with popular agent frameworks so teams aren’t locked in: CrewAI, Google ADK, LangGraph/LangChain, LlamaIndex, OpenAI Agents SDK, Strands Agents SDK, and others.

### Key Takeaways

- **Bedrock as a GenAI hub:** Amazon Bedrock centralizes access to multiple FMs and tools, so teams can experiment and ship GenAI features without standing up their own model infrastructure.  
- **Customization through prompts and data:** Techniques like zero-shot, few-shot, chain-of-thought prompting and RAG let you adapt generic models to your domain with minimal fine-tuning.  
- **Embeddings as a core primitive:** Titan Embeddings and similar models are critical for semantic search and RAG workflows, turning unstructured text into vectors that can be indexed and retrieved efficiently.  
- **Pretrained services for common use cases:** Rekognition, Textract, Comprehend, Transcribe, and others provide building blocks for vision, document, and text analytics without needing to train models from scratch.  
- **AgentCore for production agents:** AgentCore fills the gap between proof-of-concept agents and production deployments by adding runtime, memory, identity, and observability out of the box.  

### Applying to Work

- Plan to incorporate Bedrock-hosted FMs and Titan Embeddings into upcoming team projects, particularly for search and RAG-style features in internal tools.  
- Evaluate where pretrained services (e.g., Rekognition, Textract, Comprehend) can replace custom scripts in data processing pipelines to reduce maintenance overhead.  
- Consider AgentCore as a foundation for any future agentic components in the architecture, especially where secure tool usage and observability are important.  

### Event Experience

- The speakers delivered clear, practical explanations with concrete AWS service mappings, making it easy to connect high-level concepts to implementation options.  
- During Q&A, a teammate raised an architecture issue: an SNS topic handling GuardDuty findings occasionally received bursts of 1000+ alerts, risking missed processing. The recommended mitigation was to insert an SQS queue between SNS and downstream consumers to buffer spikes and ensure all events are processed.  
- Finished in the top 10 of the closing Kahoot quiz and had the chance to take photos with the speakers.  
- Formed an unofficial joint group, **“Mèo Cam Đeo Khăn”**, combining members from “The Ballers” and “Vinhomies”, which added a fun community aspect to the event.

#### Some event photos

![Mèo Cam Đeo Khăn group](/images/4-Event/Meocamdeokhan.jpg)
