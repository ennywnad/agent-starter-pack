# Agent Architecture Guide

A visual guide to understanding how AI agents work at different scales, from local development to enterprise deployment.

## Visual Learning Approach

This guide uses **progressive architecture diagrams** with clear visual hierarchy to build understanding step by step. Each diagram uses consistent color coding:

- **Blue**: Core services and primary pathways
- **Green**: Active states and successful operations  
- **Orange**: Configuration elements and manual interventions
- **Red**: Error states and security concerns

## Foundation: Single-Agent Architecture

The simplest deployment pattern - one agent, core services, clear data flow.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    FOUNDATION ARCHITECTURE                          │
│                                                                     │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │    User     │────▶│  Cloud LB   │────▶│ Cloud Run   │          │
│  │   Request   │     │  (Entry)    │     │  (Agent)    │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                  │                  │
│                                                  ▼                  │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │   Cloud     │◀────│  Vertex AI  │────▶│   Cloud     │          │
│  │  Storage    │     │  (Models)   │     │    SQL      │          │
│  │   (Docs)    │     │             │     │  (Memory)   │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                                     │
│  Components: 4 Services | Data Flow: Left→Right                   │
│  Setup Time: 15 minutes | Complexity: ⭐⭐☆☆☆ | Cost: ~$15/month │
└─────────────────────────────────────────────────────────────────────┘
```

**Key Concepts:**
- **Load Balancer**: Entry point for all traffic
- **Cloud Run**: Serverless container hosting your agent
- **Vertex AI**: Google's managed AI model service
- **Cloud Storage**: Document and file storage
- **Cloud SQL**: Persistent memory and state

## Coordination: Multi-Agent Architecture

Multiple specialized agents working together through a central coordinator.

```
┌─────────────────────────────────────────────────────────────────────┐
│                  COORDINATION ARCHITECTURE                          │
│                                                                     │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │     API     │────▶│   Agent     │────▶│   Memory    │          │
│  │   Gateway   │     │   Engine    │     │    Bank     │          │
│  │  (Traffic)  │     │  (Coord)    │     │   (State)   │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                              │                                     │
│                              ▼                                     │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │  Research   │◀────│   Agent     │────▶│   Writing   │          │
│  │    Agent    │     │    Pool     │     │    Agent    │          │
│  │  (Gather)   │     │  (Manage)   │     │  (Create)   │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                                     │
│  Components: 6 Services | Pattern: Hub & Spoke                    │
│  Setup Time: 25 minutes | Complexity: ⭐⭐⭐☆☆ | Cost: ~$85/month │
└─────────────────────────────────────────────────────────────────────┘
```

**Key Concepts:**
- **Agent Engine**: Coordinates between specialized agents
- **Agent Pool**: Manages multiple agent instances
- **Specialized Agents**: Each handles specific tasks (research, writing, etc.)
- **Memory Bank**: Shared state across all agents

## Enterprise: Multi-Environment Architecture

Production-ready setup with separate environments for development, staging, and production.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    ENTERPRISE ARCHITECTURE                         │
│                                                                     │
│  PRODUCTION ENVIRONMENT                                             │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │   VPC-SC    │────▶│   Agent     │────▶│   Vector    │          │
│  │  Perimeter  │     │   Engine    │     │     DB      │          │
│  │ (Security)  │     │   (Prod)    │     │  (AlloyDB)  │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                                     │
│  STAGING ENVIRONMENT                                                │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │     VPC     │────▶│   Agent     │────▶│   Vector    │          │
│  │   Network   │     │   Engine    │     │     DB      │          │
│  │ (Isolated)  │     │  (Stage)    │     │ (Cloud SQL) │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                                     │
│  DEVELOPMENT ENVIRONMENT                                            │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │    Local    │────▶│   Cloud     │────▶│    Test     │          │
│  │    Setup    │     │    Run      │     │  Database   │          │
│  │    (ADK)    │     │   (Dev)     │     │ (Emulator)  │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                                     │
│  Environments: 3 Layers | Pattern: Isolated Progression           │
│  Setup Time: 45 min | Complexity: ⭐⭐⭐⭐☆ | Cost: ~$350/month   │
└─────────────────────────────────────────────────────────────────────┘
```

**Key Concepts:**
- **VPC Service Controls**: Advanced security boundaries
- **Environment Isolation**: Separate resources for dev/staging/prod
- **Progressive Complexity**: More sophisticated services in higher environments
- **AlloyDB**: High-performance vector database for production

## Security: IAM Architecture

Permission and access control patterns across your cloud resources.

```
┌─────────────────────────────────────────────────────────────────────┐
│                      IAM SECURITY ARCHITECTURE                     │
│                                                                     │
│  PROJECT LEVEL                                                     │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │  Project    │────▶│  IAM        │────▶│  Service    │          │
│  │  Owner      │     │  Admin      │     │  Account    │          │
│  │  (🔴 High)  │     │  (🟡 Med)   │     │  (🟢 Low)   │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                                     │
│  RESOURCE LEVEL                                                    │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │  Storage    │────▶│  Vertex AI  │────▶│  Cloud Run  │          │
│  │  Admin      │     │  User       │     │  Invoker    │          │
│  │  (🟡 Med)   │     │  (🟢 Low)   │     │  (🟢 Low)   │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                                     │
│  Permission Flow: High → Medium → Low                              │
│  Risk Level: 🔴 Critical | 🟡 Important | 🟢 Standard             │
└─────────────────────────────────────────────────────────────────────┘
```

**Key Concepts:**
- **Permission Hierarchy**: Higher permissions include lower ones
- **Principle of Least Privilege**: Give only the minimum permissions needed
- **Service Accounts**: Non-human identities for applications
- **Risk-Based Classification**: Color coding shows permission risk levels

## Data Pipeline: RAG Architecture

How documents are processed into vector embeddings for agent context.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    DATA PIPELINE ARCHITECTURE                       │
│                                                                     │
│  SOURCE LAYER                                                      │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │  Cloud      │────▶│  Document   │────▶│  Embedding  │          │
│  │  Storage    │     │  Processing │     │  Generation │          │
│  │  (Raw Data) │     │  (Parse)    │     │  (Vectors)  │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                  │                  │
│                                                  ▼                  │
│  STORAGE LAYER                                                     │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │   AlloyDB   │◀────│   Vector    │────▶│    Agent    │          │
│  │ (pg-vector) │     │   Search    │     │    Query    │          │
│  │  (Storage)  │     │ (Retrieval) │     │  (Context)  │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                                     │
│  Data Flow: Source → Process → Store → Retrieve → Context         │
│  Processing Time: 2-5 minutes per document                        │
└─────────────────────────────────────────────────────────────────────┘
```

**Key Concepts:**
- **Document Processing**: Converting raw files into structured text
- **Embedding Generation**: Creating vector representations of text
- **Vector Storage**: Efficient storage and retrieval of embeddings
- **Similarity Search**: Finding relevant context for agent queries

## Cost Optimization Patterns

Understanding the cost implications of different architectural choices.

```
┌─────────────────────────────────────────────────────────────────────┐
│                   COST OPTIMIZATION ARCHITECTURE                   │
│                                                                     │
│  DEVELOPMENT TIER (💰 Low Cost)                                    │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │  Cloud Run  │────▶│  Cloud SQL  │────▶│   Shared    │          │
│  │  (1 vCPU)   │     │  (1 vCPU)   │     │   Memory    │          │
│  │  $5/month   │     │  $25/month  │     │  $10/month  │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                                     │
│  PRODUCTION TIER (💰💰 Medium Cost)                               │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│  │    Agent    │────▶│   AlloyDB   │────▶│  Dedicated  │          │
│  │   Engine    │     │  (4 vCPU)   │     │   Memory    │          │
│  │  $50/month  │     │ $200/month  │     │ $100/month  │          │
│  └─────────────┘     └─────────────┘     └─────────────┘          │
│                                                                     │
│  Cost Progression: $40/month → $350/month                         │
│  Performance Gain: 3x faster responses                            │
└─────────────────────────────────────────────────────────────────────┘
```

**Key Concepts:**
- **Tier-Based Scaling**: Different service levels for different needs
- **Cost-Performance Tradeoffs**: Higher costs often mean better performance
- **Resource Right-Sizing**: Choosing appropriate compute and storage sizes
- **Budget Planning**: Understanding monthly cost implications

## Next Steps

This architecture guide provides the conceptual foundation for understanding how agent systems work at scale. For hands-on implementation:

1. **Start with the basics**: Follow the [Learning Plan](learning_plan.md) for practical experience
2. **Add complexity gradually**: Build foundation → coordination → enterprise patterns
3. **Focus on security**: Always implement proper IAM and access controls
4. **Monitor costs**: Use the cost patterns to plan and budget your deployments

Remember: **Architecture follows function**. Start simple, understand the patterns, then scale up as your needs grow.