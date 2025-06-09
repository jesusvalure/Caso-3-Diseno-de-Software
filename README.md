# Caso-3-Diseno-de-Software

Members: Pablo Mesén, Alonso Durán Muñoz, Ana Hernández Muñoz, Jesus Valverde



# INDEX
- [DESCRIPTION](#DESCRIPTION)
- [STRATEGY AND PLANNING](#STRATEGY-AND-PLANNING)
- [DEFINITION OF REQUIREMENTS](#DEFINITION-OF-REQUIREMENTS)
- [SYSTEM ANALYSIS](#SYSTEM-ANALYSIS)
- [LEGAL AND REGULATORY FRAMEWORK](#LEGAL-AND-REGULATORY-FRAMEWORK)
- [STACK](#STACK)


# DESCRIPTION

# STRATEGY AND PLANNING
## Roadmaop
![Agile Product Roadmap](https://github.com/user-attachments/assets/41e78649-b798-461c-a0dc-3f73e239cb19)
## Milestones
- M1: Validated architecture

- M2: Core technical infrastructure deployed (data lake, backend, encryption)

- M3: Secure user registration system with advanced validation and IP control

- M4: First datasets published with granular control, metadata, and pricing

- M5: AI-powered, non-downloadable data exploration dashboards operational

- M6: Ecosystem validated through security audits and regulatory compliance

- M7: 10+ integrated institutions, 100+ active datasets, first monetization revenue
## Technical Team
| **Role**                  | **Quantity** | **Responsibilities**                                                       |
|---------------------------|--------------|-----------------------------------------------------------------------------|
| Cloud Solutions Architect | 1            | Designs cloud infrastructure, security, scalability                        |
| DevOps Engineers          | 2            | Deployment automation, CI/CD, versioning                                   |
| Backend Developers        | 3            | API development, encryption, access control, authentication                |
| Frontend Developers       | 2            | User portals (registration, dashboards, admin interface)                   |
| Security Specialists      | 2            | Key management, MFA, audits, legal compliance                              |
| AI & ETDL Specialists     | 2            | Document validation, data flow automation, AI dashboards                   |
| Data Engineers            | 2            | Modeling, metadata, data quality                                           |
| Product Manager           | 1            | Agile delivery coordination, documentation                                 |
| UI/UX Designer            | 1            | Portal and backoffice interface design                                     |
| Legal & Compliance Advisor| 1            | Legal analysis, data protection, IP/data ownership                         |

## Comprehensive Strategy
## KPIs and Metrics
## Risk assessment

## Deployment and Operations Strategy

The deployment strategy for Data Pura Vida utilizes Blue-Green Deployment to ensure zero-downtime updates, meeting the 99.9% SLA and supporting scalability for millions of records and thousands of concurrent users. New versions are deployed to a "Green" environment, mirroring the production "Blue" environment, using AWS Fargate for the monolithic Node.js/Express backend and React Native frontend. GitHub Actions and Terraform automate CI/CD, provisioning infrastructure like Snowflake, AWS S3, and VPC endpoints. Comprehensive tests (unit, integration, security via OWASP ZAP, and load) validate performance (<200ms query latency, <0.1% errors) in Green. AWS WAF restricts access to Costa Rica-based IPs, and a Node.js service with AWS KMS manages tripartite keys (one part with Data Pura Vida, two with custodians). Traffic shifts to Green via AWS Application Load Balancer after validation, with Blue retained for 24 hours for rollback, ensuring <2-hour recovery. This approach supports future microservices migration and complies with Law 8968, GDPR, and ISO 27001.

## Monitoring and Operations

Observability: AWS CloudWatch captures structured logs from all system layers, complemented by Snowflake query history for data access audits, ensuring traceability per Law 8968 and GDPR. CloudWatch Metrics track latency, errors, traffic, and per-user/entity dataset usage. AWS SNS sends alerts for downtime, errors, or security incidents (e.g., unauthorized IP access). Amazon QuickSight dashboards provide real-time insights into system health, dataset usage, and backoffice operations like user management and audits.

High Availability: Snowflake’s multi-cluster architecture and Fargate auto-scaling handle usage spikes and 10TB/year data growth. AWS Application Load Balancer with WAF distributes traffic. Daily full backups and 4-hour incremental backups in S3 with versioning, plus Snowflake snapshots, ensure data integrity. Automated disaster recovery playbooks using Terraform and S3/Snowflake restore the system in <2 hours, tested quarterly. Snowflake Snowpark drives AI for data normalization and dashboards, with AWS SageMaker as a fallback, and CloudWatch optimizes costs for Snowflake and Fargate usage.

# DEFINITION OF REQUIREMENTS

## Functional Requirements

### User Management and Registration

- Allow registration of natural persons, legal entities, institutions, chambers, groups, and companies, and adapt the system (forms) based on the user type.

- Digital identity, biometrics, liveness detection, and MFA (Multi-Factor Authentication).

- Assign tripartite security keys to organizations for access delegation/revocation.

- Capture IBAN and/or credit card data during registration.

- Email notification system.

- Generate symmetric and asymmetric keys with a tripartite system.

### Data Management

- Support for Excel, CSV, JSON APIs, SQL and NoSQL databases.

- Define data as public/private, free/paid, temporary/permanent.

- Access control by institution, individual, or specific group (granular control).

- Allow selection of specific fields to encrypt.

- Specify columns that relate datasets to each other.

- Extraction, transformation, cleaning, context detection, modeling, and loading (ETDL).

- Avoid duplication and optimize existing relationships.

- Configuration of differential fields and update frequency.

### Commercialization and Payments

- Free data up to a certain size, with charges for additional space.

- Platform fee/commission on paid data based on size and duration.

- Multiple payment methods: credit cards, debit cards, and national mechanisms.

- Automatic permission assignment upon payment confirmation.

- Consumption control, real-time monitoring of paid data usage.

### Visualization and Analysis

- Custom dashboards, built manually or through AI prompts.

- Multiple visualizations: tables, graphs, counts, trends, and predictions.

- Share dashboards among users or allow internal public visibility.

- The system must implement data delivery mechanisms that minimize the possibility of indirect extraction of information, especially for unauthorized uses such as AI model training. When necessary to deliver visual representations of data, formats like SVG or PDF may be considered, as long as they prevent automated reconstruction or reverse engineering of the underlying data.

- Log all transactions and data usage in a user-accessible history for consultation and internal audit.

### Backoffice

- User management: maintenance of identities, memberships, and roles.

- Configuration of rules, formats, validations, and structures by entity type.

- Administration of API connectivity, databases, and external callbacks.

- Key management: revocation and regeneration of security keys.

- Full audit trail: details by user, action, date, and effect.

- Usage reports, access logs, consumption data, metrics, data quality, and anomalies.

- Operational monitoring: service status, tasks, transfers, and processes.

## Non-Funtional Requirements

### Performance

- The dashboard query engine must deliver results in under 200 milliseconds for 95% of requests under normal load.
  
- The platform shall support at least 200 concurrent users with no noticeable performance degradation. All of this the fifth year of operation.
  
- Data ingestion pipelines must process files up to 1GB in size within 2 minutes, including ETDL stages.

### Scalability

- The infrastructure must support automatic horizontal scaling in response to usage spikes.

- The data lake architecture must accommodate growth of up to 10TB per year without manual intervention.

- The system must allow onboarding of new data providers without modifying core services.

### Reliability

- The system must not exceed 43.2 minutes of downtime per month, aligning with a 99.9% SLA.

- All transactions must be ACID-compliant, and service errors must trigger automated retries with rollback capabilities.

- Full backups of all datasets and user metadata must be taken daily, with incremental backups every 4 hours.

- In the event of a critical failure, full system recovery must be achieved within 2 hours, using predefined playbooks and automated infrastructure recovery.

### Availability

- The system must maintain 99.9% availability, with high availability configurations and load balancers.

- The system infrastructure (including core services, APIs, and Snowflake-based data operations) must demonstrate a minimum mean time between failures of 30 days, ensuring fault tolerance at both the compute and storage layers.

- In the event of any service interruption or system failure, the system must be recoverable and operational within a maximum mean time to repair of 2 hours, supported by automated diagnostics and recovery scripts.

- Scheduled maintenance activities must:
	- Be limited to predefined windows.
	- Be communicated to stakeholders at least 72 hours in advance.
	- Include health verification steps and after the execution to ensure service continuity.

- The system must include Snowflake’s automatic multi-cluster architecture to ensure high availability and fault isolation for queries and pipelines and real time monitoring and alerting to trigger failover workflows.

### Security 

- All data in transit and at rest must be enceypted using AES-256. All APIs must enforce TLS 1.3 for secure communications.

- PENDIENTE HABLAR SOBRE auth y verification

### Usability

- Screen reader support, keyboard navigation, and contrast options must be available to ensure compliance with inclusive design principles.

- Some of the inclusive design principles to follow are: 
	- Give control: Allow users to personalize their experience (font size, colors, contrast)
	- Offer choices: Offer multiple choices to interact (voice, clicks, keyboard)
	- Provide equivalent experiences: Ensure the same experience to all users in despite of their capabilities

- Use of UX principles for a better user interaction with the UI.
	- Consistency: Use familiar patterns, layouts, and terminology throughout the interface so users can predict interactions and learn quickly.
	- Feedback: Provide immediate, clear responses to user actions (e.g., button clicks, form submissions, errors) so users understand what is happening.
	- Simplicity: Eliminate unnecessary steps, clutter, or features. Focus on helping users complete tasks with minimal effort and cognitive load.
	- Visibility of System Status: Keep users informed about what’s going on with timely and appropriate status indicators (e.g., loading spinners, progress bars, confirmation messages).

### Maintainability

- All system components must emit structured logs to a centralized logging system. Metrics must be exposed via Prometheus, with real time dashboards in Amazon Quicksight.

- Source code must be managed using Github with GitFlow branching. All deployments are automated via CI/CD pipelines with infrastructure defined via Terraform.

- All code must adhere to industry recognized standards and best practices:
	- Follow the SOLID principles for maintainable software.
	- Maintain at least 90% unit test coverage across all business logic and services.
	- All pull requests must be peer-reviewed, and merged only after passing all automated tests.
	- Commit messages must follow the Conventional Commits specification for traceability.

### Interoperability

- All data exchange and system interaction must comply with open standards to ensure broad interoperability across institutional and technological boundaries.

- The system must support seamless integration with:
	- External public data systems (government registries, public entities databases)
	- Private sector API’s
	- Both SQL and NoSQL connectors for ingesting structured and semi-structured data.

- API’s must be:
	- RESTful, stateless, and resource-oriented.
	- Secured with rate limiting, IP whitelisting, token-based auth, and MFA.
	- Equipped with detailed versioned documentation (e.g., Swagger UI).
	- Designed to expose only the minimum viable data based on roles and permissions (RBAC/RLS).

### Compliance

- The system must fully comply with Law 8968 – Protection of the Person Regarding the Processing of Personal Data (Costa Rica), including provisions for:
	- Informed consent and purpose limitation.
	- Data subject rights such as access, rectification, and deletion.
	- Registration of personal data databases with PRODHAB (Agencia de Protección de Datos de los Habitantes).

- All personal and sensitive data handling must adhere to the General Data Protection Regulation (GDPR).

- The platform must implement and maintain an Information Security Management System (ISMS) aligned with ISO/IEC 27001.

- OECD Recommendation on Data Governance for the Public Sector. Data Pura Vida must reflect principles of:
	- Openness and reusability of public data.
	- Trust, transparency, and accountability in data use.
	- Clear roles and responsibilities for data stewardship.

- Related to security industry standards to follow:
	- Use of TLS 1.3 for encrypted communications.
	- End-to-end encryption of sensitive data using AES-256.
	- Implementation of Role-Based Access Control (RBAC) and Row-Level Security (RLS).

### Extensibility

- The platform must allow modular addition of new AI models, data connectors, and dashboard templates without affecting existing operations. APIs and services must be designed to support future migration to microservices.

### Documentation

- End-user guides must be provided Spanish, accessible directly within the portal.

- System administration manuals, backup and recovery guides, and access control procedures must be maintained in a secure internal wiki.

- Full API documentation (Swagger), architecture diagrams, CI/CD guidelines, and code style references must be available in the repository.

- Documentation must be version-controlled alongside the codebase and updated with each release as part of the definition of done in Scrum tasks.

# SYSTEM ANALYSIS
## DETAILED DECOMPOSITION BY COMPONENTS


# LEGAL AND REGULATORY FRAMEWORK

## Regulatory Compliance and Privacy Policies
For years, Costa Rica has faced a significant structural limitation: the absence of a centralized data system that facilitates access, analysis, and utilization of information by diverse actors. Currently, there is no national ecosystem that allows individuals, public institutions, state branches, social organizations, and the private sector to share, reuse, and market information in a structured manner. This fragmentation has hindered evidence-based decision-making, slowed institutional processes, and limited the development of innovative solutions that could emerge from the strategic use of data.
One of the main obstacles to overcoming this gap is not only technical but also political and institutional. Many state organizations do not feel comfortable sharing information with other government entities or private actors, whether due to mistrust, institutional jealousy, concerns about misuse, or a lack of regulatory clarity. This reluctance creates data silos that prevent the construction of an integrated view of the country and limits the potential for cross-sector solutions. Faced with this challenge, DataPuraVida proposes a flexible and controlled approach to overcome this political problem. Participating organizations would not be required to share all their data; instead, they would be allowed to choose which data to share, under what conditions, and with whom. This creates a transparent and regulated data market, where institutional autonomy is respected, but collaboration is encouraged through clear rules, incentives, and trust. By offering control, privacy, and traceability mechanisms, the system reduces political resistance and lays the groundwork for true data governance at the country level.

## User-centric data governance and regulatory compliance
Link to view Law 8968: http://www.pgrweb.go.cr/scij/Busqueda/Normativa/Normas/nrm_texto_completo.aspx?param1=NRTC&nValor1=1&nValor2=70975
The system must be developed in compliance with the principles established in Law 8968. This includes:
- Informed consent: All collection and processing of personal data must have the free, specific, and documented authorization of the data subject, with the possibility of easy revocation.
- Clear purpose: Each use of data must be justified, informed, and limited to its original purpose.
- Data subject rights: Interfaces will be enabled so users can securely review, correct, or request deletion of their data.
- Quality and updating: The system will incorporate automatic validations and periodic reviews to keep the data accurate and relevant.
- Protection of sensitive data: If information such as health or religious information is handled, enhanced security measures will be applied and explicit consent will be required.
- Data processing responsibility: Each database must have a clearly identified and trained person responsible for ensuring legal and technical compliance.
- Robust security: Encryption, access control, and monitoring protocols will be implemented to prevent security breaches.

## Integration of international standards
Link to view GDPR (Spanish): https://eur-lex.europa.eu/legal-content/ES/TXT/?uri=CELEX%3A32016R0679
In addition to national legislation, DataPuraVida aligns with the European Union's General Data Protection Regulation (GDPR), which establishes advanced standards for the ethical handling of information. Among its key contributions to the system:

- Privacy by design and by default: The system will only collect the minimum data necessary, ensuring anonymization, access segmentation, and minimization through its architecture.
- Data Impact Assessments (DPIA): A formal risk analysis will be conducted when the system processes personal data on a large scale or sensitive data.
- Data Protection Officer (DPO): It is recommended to designate a person responsible for regulatory compliance, technical advice, and contact with users.
- Portability: Users will be able to obtain and transfer their data in standard formats, fostering their autonomy.
- Incident Management: Mechanisms will be implemented to detect and report security breaches within 72 hours, meeting transparency and immediate action requirements.
- Accountability: The system will document all actions involving personal data, conducting audits and maintaining traceability to demonstrate compliance. Fines of up to €20 million or 4% of global annual turnover, whichever is greater. According to the following article: https://time.com/5290043/nazi-history-eu-data-privacy-gdpr/?.com

## Security and interoperability as pillars
The implementation of ISO/IEC 27001 will ensure a rigorous approach to information security. The following will be established:
- Clearly defined roles and permissions.
- Cryptographic controls and strong authentication.
- Regular audits to verify data integrity, confidentiality, and availability.
- Continuous improvement plans for security management.

The OECD Guidelines on Data Governance provide a holistic view. For DataPuraVida, this means:
- Data quality and interoperability: Establishing standards for metadata, formats, and version control.
- Clear and accountable governance: Defining inclusive governance structures that promote the participation of public, private, and social sectors.
- Data lineage documentation: Enabling the tracing of data origin, use, and transformations.
- Transparency and public trust: Ensuring that all system processes are visible and auditable.

DataPuraVida is not simply a technological infrastructure, but a platform for the political, institutional, and social transformation of the country. By enabling voluntary yet secure management of data exchange and complying with the most demanding national and international standards, the system can break decades of fragmentation and become the catalyst for an economy based on knowledge, innovation, and trust. Data governance is no longer an option, but a necessity for the sustainable, transparent, and equitable development of Costa Rica.


# STACK

- Amazon Web Service as the designated Cloud Service

- Snowflake for data analytics and data processing, also for for MFA, IP whitelist and token validation (session validation)

Frontend:

    React Native 18.2.x – Provides high performance and scalability for web and mobile interactions.

Backend:

    Node.js 20.x: Handles all incoming REST requests. Connects to the PostgreSQL database. Implements general business logic (authentication, user management, file uploads, task creation) implementing external services such as AWS services.

    Express 4.x: Web framework for Node.js, used to handle REST, manage middleware, routing, and request/response lifecycle.

    REST: For structured, not state-dependant operation and service-oriented operations. Authentication and registration, use of AWS services, etc... 

Database:

    Snowflake --> Definir más 

AI & Machine Learning:

    Snowflake can be integrated with LLM's, in relation with the choosen model of the LLM a training for ETDL flow management is required. This AI will be used as well for documents revision.
    Reference Link: https://www.youtube.com/watch?app=desktop&v=9FejjGVZrPg&t=0s

Cloud & Hosting:   
 
    Amazon Web Services: Ensures good communication and compatbility with Snowflake and multiple useful services. 

DevOps & CI/CD:

    GitHub Actions: Automates integration and deployment workflows.

Quality Assurance:

  
## Frontend design specifications

## Data

## Third-Party 

## Cloud

## Protocols

# FRONTEND

# BACKEND

## APPROACH

### API Design and Architecture

The chosen approach for the API architecture is a Monolithic architecture. Meaning, the entire application (UI, business logic, data access, integrations) is built, deployed and managed as a single, cohesive unit. Components within the monolith typically communicate directly through method calls or internal APIs.
The monolith will run on AWS Fargate; this moves closer to serverless for the container execution, as AWS manages the underlying EC2 instances for Fargate. However, the system takes advantage of AWS services for specific functionalities like authentication, storage, ETL, monitoring and security; this enables modern scalability, resilience, security, and observability, common features in cloud-native and decoupled architectures.
The API will follow the principles of Representational State Transfer (REST). Rest APIs use standard HTTP methods (like GET, POST, PUT, DELETE) to communicate with the servers, enabling clients to access and maniúlate data. Taking in consideration that “Data Pura Vida” is mainly a data consultation service the approach is ideal. For example:

- CRUD operations
- File uploads 
- Authentication and user management

### Serverless, Cloud, On-Premise, or Hybrid?

```
Hardware Demands and Cloud Machine Types
Impacts frameworks, libraries, and programming languages

Service vs Microservice - Planning of migrate to microservice
API Gateway (Security & Scalability)?

Definir arquitectura monolítica con migración a microservicios
Implementar versionamiento de endpoints
Crear módulos de autenticación y autorización
Diseñar gestión de credenciales y cifrado
Implementar auditoría y trazabilidad completa
Crear endpoints para gestión de datasets
```

#### Internal Layers Handling Requests/Responses

#### 1. Handler Layer
This is the entry point for all the HTTP requests (REST), exposes HTTP endpoints, orchestrates middleware execution, delegates business logic to the Service Layer, applies middleware for cross-cutting concerns, returns formatted HTTP responses.

Design Patterns: Controller Pattern, Template Method Pattern (via BaseHandler).
Principles Applied: Code structure follows the Single Responsibility Principle by strictly separating concerns (HTTP, auth, business logic), and encourages DRY via shared base classes and middleware use.
, DRY (shared base logic), Open/Closed Principle (extensible handler logic).

- `BaseHandler`: initializes a `middlewareChain` and a `logger`, exposes methods `executeMiddleware()`  to run pre- and post-processing hooks, `createSuccessResponse()` to build a standard OK response with payload, and `createErrorResponse()` to format errors into appropriate HTTP messages. Concrete handlers override the abstract `handle()` method to implement endpoint-specific behavior.

- `AuthHandler` focuses on user authentication workflows. Its key methods—`login()`, `logout()`, `validateSession()`, and `refreshToken()`—delegate credential checks to `AuthService`, record actions in `AuditService`, and manage session state via `SessionService`. Errors like invalid credentials or expired tokens are captured and returned through the base error flow.

- `DatasetSharingHandler` manages granting and revoking permissions on datasets. Methods such as `shareDataset()` and `revokeAccess()` call `AccessControlService` to update policies in Snowflake, while `setRowLevelCriteria()` and `validateRowAccess()` enforce fine-grained security via `RowLevelSecurityService`.

- `DatasetHandler` orchestrates dataset lifecycle operations: initialization (`initUpload()`), chunked uploads (`uploadChunk()`, `finalizeUpload()`), status queries (`getUploadStatus()`), and cleanup (`cancelUpload()`, `deleteDataset()`). It uses `UploadSessionService` to track progress, `StorageService` for S3 interactions, `ValidationService` to verify data integrity, and `DataCipherService` for encryption.

- Other Handlers such as `AIHandler`, `SubscriptionHandler`, `ViewHandler`, `LogHandler`, and `AccessHandler` follow a similar pattern: they delegate domain-specific operations to appropriate services (AIService for model training in AIHandler, PaymentService in SubscriptionHandler) and encapsulate all HTTP-specific logic (parsing inputs, handling exceptions, formatting outputs).


`Object design patterns interact with requests or any other trigger`

#### 2. Middleware Layer

Pre-/post-processing for handlers, authentication, logging, request parsing, could be optional or mandatory depending on context. The Middleware layer implements cross-cutting concerns by chaining together small, focused components:
Design Patterns: Chain of Responsibility Pattern, Strategy Pattern (different middleware types)
Principles Applied: Separation of Concerns, DRY, Open/Closed Principle
Each middleware acts as an independent processing step, allowing flexible reuse across different request types. The Chain of Responsibility pattern allows requests to pass through a dynamically composed stack of middleware functions.
Each middleware implements a common interface with `before()` and `after()` hooks, making it easy to plug new logic without modifying existing handlers.
The use of specialized components (AuthenticationMiddleware, LoggingMiddleware, etc.) cleanly separates concerns, adheres to the Open/Closed Principle by allowing extension without modifying the core framework, and promotes DRY by avoiding repeated inline validations.

- `MiddlewareChain` holds an ordered list of `Middleware` objects. When a request arrives, `BaseHandler.executeMiddleware()` invokes each middleware’s `before(request)` hook; after the handler runs, any defined `after(response)` hooks execute in reverse order.
- `AuthenticationMiddleware` checks incoming JWTs or session tokens, aborting unauthorized requests early.
- `ValidationMiddleware` uses predefined JSON schemas or parameter definitions to validate request payloads, returning detailed Bad Request errors if validation fails.
- `SecurityMiddleware` enforces payload encryption, CSRF (a malicious exploit) protection, or signature verification as needed.
- `LoggingMiddleware` logs request metadata, user IDs, and trace IDs at the start and end of each request. It writes to the centralized logger, ensuring all events are captured.
- `RateLimitMiddleware` enforces per‑user or per‑IP call quotas, returning “Too Many Requests” when thresholds are exceeded.
- `ErrorHandlingMiddleware` wraps the entire chain, catching unhandled exceptions, logging stack traces, and converting them into standardized error responses via `BaseHandler.createErrorResponse()`.

#### 3. Service Layer
Contains core business logic, coordinates between repositories and other services, validations and transformations, abstracts away direct interactions with data stores and third‑party APIs.
Design Patterns: Service Layer Pattern
Principles Applied: Single Responsability Principle, DRY, Dependency Inversion Principle (via repository abstractions), Encapsulation
The service layer abstracts and centralizes domain-specific operations, removing complexity from handlers. It coordinates interactions with repositories, external APIs, and internal workflows.
For example, `DatasetService.finalizeUpload()` may orchestrate encryption (`DataCipherService`), validation (`ValidationService`), and storage operations (`StorageService`)—this demonstrates Facade use by simplifying these dependencies into a single method call.
The Dependency Inversion Principle is applied by injecting interfaces (repositories, clients) into services rather than hardcoding implementations, making components testable and interchangeable. Each service follows SRP by focusing on one area (uploads, AI, auditing), and code reuse is reinforced via shared helpers/utilities, following DRY.

- `AuthService` manages user credentials, token generation, and verification. It interfaces with an RDS or NoSQL store for user records and signs JWTs for stateless authentication.
- `SessionService` tracks active sessions, refresh tokens, and expiration times, ensuring users can seamlessly obtain new access tokens without re‑authentication.
- `AccessControlService` evaluates policies for dataset and row‑level permissions. It generates policy documents, caches decisions, and interacts with Snowflake grants or IAM rules.
- `DatasetService` handles metadata CRUD operations on Snowflake, abstracting SQL queries, schema migrations, and versioning of dataset definitions.
- `StorageService` orchestrates S3 multipart uploads, presigned URL generation, and handles retries on network failures.
- `DataCipherService` wraps encryption and decryption routines using AWS KMS or custom key management, ensuring all persisted data is encrypted at rest and in transit.
- `UploadSessionService` tracks the state of multi‑part uploads, persists session records, and implements idempotent retry logic.
- `ValidationService` verifies data format, file hashes, and schema conformance before datasets are finalized.
- `DigitalSignatureService` signs dataset artifacts to prevent tampering and provides cryptographic proof of integrity.
- `IntegrityMonitor` scans stored data periodically for corruption or unauthorized changes, issuing alerts through `AuditService`.
- `AIService` provides a high‑level API for training, evaluating, and deploying ML models. It submits jobs to AWS Batch or SageMaker and stages artifacts in S3.
- `PipelineService` orchestrates long‑running workflows using AWS Step Functions or custom state machines, handling retries and error states.
- `PaymentService` integrates with Stripe (or other gateways), handles webhooks, and ensures PCI‑compliance for payment operations.
- `SubscriptionService` manages subscription plans, billing cycles, and integrates usage metrics with billing rates.
- `ViewService` and `MetadataService` power dashboard creation and schema introspection for the front end.
- `LogExportService` retrieves logs from CloudWatch or Elasticsearch and exports them to S3 or third‑party analytics tools.
- `AuditService` centralizes event logging with user, action, and trace ID, producing a complete audit trail for compliance.

#### 4. Repository Layer 
This layer abstracts direct interactions with external storage and identity backends, providing consistent `IRepository` interfaces for various persistence mechanisms. Lambda Handlers and Services use these repositories for low-level CRUD and operational calls.
Design Patterns: Repository Pattern, Facade Pattern (MainRepository), Adapter Pattern (external services)
Principles Applied: Interface Segregation Principle, Single Responsability Principle, Dependency Inversion, DRY

- `MainRepository` acts as a factory or facade, exposing specific sub-repositories:
- `S3Repository` for object storage
- `SFRepository` for Snowflake operations
- `AWSVaultRepository` for secrets management
- `CognitoRepository` for user identity and token management
- `S3Repository` implements `IRepository` to manage S3 objects:
	- `putObject()`, `getObject()`, `deleteObject()`, `listObjects()`, `copyObject()`
	- Multipart upload support: `initiateMultipartUpload()`, `uploadPart()`, `completeMultipartUpload()`, `abortMultipartUpload()`
	- `generatePresignedUrl()` and `headObject()` for preflight checks
	- Internally uses `s3Client` (AWS.S3), `bucketManager`, and `multipartUploadManager` for robust uploads.
- `SFRepository` implements data warehouse operations:
	- Generic query execution via `executeQuery(sql, params)`
	- Specialized utilities: `createTemporaryTable()`, `bulkLoadFromStage()`, `cloneTable()`, `getQueryProfile()`, `executeAIQuery()`
	- CRUD operations on warehouse artifacts (roles, procedures) through standardized methods.
- `AWSVaultRepository` manages secrets and dynamic credentials in a secure vault:
	- `writeSecret()`, `readSecret()`, `deleteSecret()`, `listSecrets()` for static secrets
	- Policy management: `createPolicy()`, `attachPolicy()`
	- Dynamic credential lifecycle: `generateDynamicCredentials()`, `renewLease()`, `revokeLease()`
	- Relies on `VaultClient`, `VaultAuthManager`, and `SecretManager` internally.
- `CognitoRepository` encapsulates AWS Cognito user pool operations:
	- `authenticateUser()`, `validateToken()`, `refreshToken()` for session management
	- User management: `createUser()`, `getUserInfo()`, `updateUserAttributes()`, `deleteUser()`, `resetPassword()`, `confirmUser()`
	- Uses `cognitoClient`, `userPoolManager`, and `tokenValidator` under the hood.

Necessity of AWSVaultRepository and CognitoRepository:
`AWSVaultRepository` is critical if your system integrates with a central secrets vault for rotating database credentials, API keys, or encryption keys. If you rely solely on AWS IAM roles and KMS, you may simplify by removing it.
`CognitoRepository` is necessary only if you use AWS Cognito for user identity. If authentication is entirely custom or via another provider, you can omit this repository and adapt `AuthService` to your chosen auth backend.


#### 5. Security Layer

`Object design patterns interact with requests or any other trigger`

#### 6. AI Layer

`Object design patterns interact with requests or any other trigger`

## BACKOFFICE PORTAL WEB
```
Diseñar interfaz de administración de usuarios
Crear gestión de reglas de carga de datos
Implementar administración de conectividad externa
Diseñar sistema de auditoría y reportes
Crear monitoreo operativo del sistema
Implementar RBAC (Role-Based Access Control)
```

## Data Layer Design

### Data Architecture & Storage

#### a) Data Topology

#### b) Big Data Repository - Data Lake

#### c) Database Engine

#### d) Tenancy and Data Security

### Object-Oriented Design - Programming

### Object-Oriented Design - Programming
#### a) Transactionality
#### b) Use of ORM
#### c) Connection Pooling
#### d) Use of Cache
#### e) Drivers
#### f) Data Design

```
Qué hacer:
Diseñar arquitectura de almacenamiento masivo
BatchLoad
Implementar IA para normalización de datos
Crear sistema de detección de duplicados
Diseñar gestión de cargas delta
Implementar cifrado en reposo y en tránsito
Crear sistema de trazabilidad de datos
Anotaciones del profesor:
Batch o stream
Evitar repetir datos con LLM? 
Con contexto, se puede dar contexto para crear el modelo dinámico de datos. (Ver patrones de diseno de agentes para AI [Ejercicio 9])
Alteracion de diseno de tablas inteligentemente.
ETL = Extract-Transform-Design-Load
Unificación de datos
Merge de datos
Actualización parcial y deltas
Temas a investigar:
Identidad Digital
DID: Decentralized Identificaction
Diseno de procesos
Diseno de servicios
Tipos de registro: 
Diferente documentación
Diferente proceso de registro
Validaciones diferente: 
Validacion por AI 
Validacion de formatos de datos
Scanners de documentos (Naciona, extrangeros, 3rd party services)
Diseno de DB: Cada cloud tiene al menos una maquina de workflows
```


### Datalake

```
Ideas para el Data Lake, aqui hay un enlace para tener una definición concreta: https://www.geeksforgeeks.org/what-is-data-lake/

A continuación hay varios de los puntos del apartado que investigue y trate de buscar soluciones:

- La API debe desarrollarse en la misma tecnología cloud utilizada para los portales web del sistema.

Se puede utilizar lambda functions, cloud functions, azure functions.
https://docs.aws.amazon.com/lambda/latest/dg/welcome.html

- Toda interacción con la API debe estar protegida por mecanismos de acceso como whitelist de IPs, validación de tokens y MFA.

Esto lo puede manejar snowflake https://docs.snowflake.com/en/user-guide/security-mfa

- Las operaciones API deben cubrir: autenticación, validación de identidad, gestión de usuarios, operaciones sobre datasets, llaves de acceso, métricas, y procesos administrativos

AWS VPC endpoints sirven para whitelisting, etc
AWS secrets para validacion y segmentacion de info, credenciales, tokens

- La lógica de negocio debe garantizar trazabilidad, cumplimiento legal, y control de cada transacción realizada dentro del sistema

Snowflake tiene historial por usuario, por rol, warehouse, etc. Son los query history de snowflake
Para la parte legal, hay que asegurar que todo el historial se guarda, que hay un warehouse especifico, desde un IP especifico....

- Se deben incluir endpoints para gestionar accesos temporales, revocación de permisos, y control granular por rol y contexto.

Snowflake con aws, con middleware.
https://aws.amazon.com/financial-services/partner-solutions/snowflake/

- Aunque se llame “datalake”, puede ser cualquier infraestructura moderna que permita almacenamiento, consulta y gestión masiva de datos

AWS s3 para guardar los datos raw y luego en snowflake con los stages que jala los datos y los guarda. AWS Glue guarda todo raw y luego se pasa a snowflake ya que snowflake no puede almacenar datos raw, pasa por un proceso de ETL a snowflake, estructurado o semiestrucurado. Almacenamiento dentro de GLUE y gestion en snowflake

- Debe soportar millones de registros, miles de usuarios concurrentes, y un crecimiento dinámico de la información.

Snowflake, warehouse mas grande = crecimiento vertical, mas clusters = crecimiento horizontal.

- Usar inteligencia artificial para normalizar los modelos de datos, rediseñarlos según uso y relacionarlos automáticamente con datasets existentes

Snowflake for AI. https://www.snowflake.com/en/product/ai/

- Detectar y evitar duplicidad de datos durante cargas y transferencias

bronce layer (Data raw), silver layer (Para los devs, ing de datos para manipular), gold layer (para el end user). Duplicidad en el silver layer tener un master antes de echar los datos al master del silver layer se verifica si ese dato ya existe y asi no se duplica. AI es una alternativa, pero tambien estan los orquestadores como Apache Airflow (recomendado porque es agnostica que sirve con snowflake), AWS tambien tiene.

- Controlar y gestionar cargas delta, identificando diferencias entre cargas anteriores y actuales

Snowflake y tablas incrementales. Si se actualiza/manipula datos como manter la informacion intacta y no duplicada. Load data that hasn't been loaded yet. Snapshots anteriores, cargas las que van entrando. No es el orquestador. Snapshot de snowflake: https://docs.snowflake.com/en/sql-reference/sql/create-snapshot
dbt cargas delta: https://docs.getdbt.com/docs/build/incremental-models

- Ser eficiente en operaciones de merge de datos, sin perder integridad ni contexto

De nuevo, Bronce, silver y gold layer. Es conservar informacion relevante del goldlayer basicamente. Nulos, ceros, casos atipicos, incongruentes. Esto se maneja con reglas de negocios definidas entre nosotros y las instituciones, pero basicamente definidas anteriormente.

- Llevar trazabilidad continua de datos usados, no usados, descartados y archivados

Trazabilidad de las capas. De la gold toma tablas de la silver y asi, por medio de nodos se puede ver cuales son usados, no usados, descartados y archivados. Gobernanza las las personas deben documentar bien que es lo que hacen. Es como un arbol con nodos e hijos.
DBT gestor de datos, sirve para esto con gobernanza

- Tener monitoreo en tiempo real de estado, salud, tráfico, errores, cuellos de botella y uso por entidad o usuario

Entidad o usuario con snowflake sirve salud, tráfico, errores, cuellos de botella con orquestadores (airflow)

- Permitir múltiples niveles de acceso con control lógico, por usuario, entidad, o tipo de dato

Snowflake

- Implementar RBAC (control de acceso basado en roles) y RLS (restricción a nivel de fila) para segmentación fina

Snowflake https://docs.snowflake.com/en/user-guide/security-access-control-overview

- Toda la data sensible debe estar cifrada en reposo y en tránsito, y sus accesos siempre deben dejar registro auditable

Cualquier cosa que se haga en snowflake se registra y permite encriptacion

- Restringir IP whitelist importante,

hacer el acceso a datos por warehouses para accceder a datos en particular solo en esas tablas y ya.
Limitacion o restricciones a nivel de rol. 

- Por ultimo cifrado, encriptado, hashes, etc.
  https://docs.snowflake.com/en/sql-reference/functions/encrypt 



Buenas noches compañeros y 
@vsurak
.  A continuación voy a hacer una breve explicación de la investigación/análisis que he realizado en relación al diseño solución del caso 3 y algunas sugerencias hechas anteriormente por el profesor. A manera de aclaración la investigación tiene vinculo principalmente con la parte de "Pura Vida DataLake".
Primeramente, proporcionar dos videos que me parecieron de mucha utilidad para entender mejor Snowflake y como integrarlo a nuestro diseño:
1 - Getting Started - Architecture & Key Concepts: https://youtu.be/GtVwChmxdpw?si=4B9XBEY1BkyU4yK4
2- Getting Started: Introduction To Snowflake Virtual Warehouses: https://youtu.be/TeD5zshkdjY?si=9Vz-Y4WBu6H82gci
El segundo video se adentra en uno de los conceptos principales de SF, y el cual se describe en el primer video como una parte fundamental de su arquitectura. Ahora se presentarán algunas ideas para un par de puntos del diseño.

- Debe soportar millones de registros, miles de usuarios concurrentes, y un crecimiento dinámico de la información
Como se mencionó en observaciones pasadas SF solamente ofrece algoritmos y un diseño efectivo, por esto hay diferentes servicios de AWS que podrían servir. Se contempla AWS Lake Formation para construir, asegurar y administrar un data lake centralizado sobre Amazon S3 (Simple Storage Service), el cuál es sumamente escalable y no necesita predefinir limites de espacio.
Lo anterior se basa en separar responsabilidades para que el almacenamiento principal esté en cloud y SF se pueda centrar en otras cosas como análisis y procesamiento de datos a gran escala.
- El sistema estará basado inicialmente en servicios monolíticos con posibilidad de migrar a una arquitectura de microservicios en el futuro
Se tienen dos propuestas para este aspecto:
	1- Utilizar patrones como el "Modular Monolith", donde cada dominio (autenticación, compartición, 	visualización) se desarrolla como un módulo autónomo dentro del mismo despliegue.
	2- Implementar una arquitectura por capas separando handlers, servicios y repositorios.
- Se debe implementar versionamiento en los endpoints de la API y mantener compatibilidad hacia atrás en la medida posible
Hay diferentes prácticas como opciones en esté aspecto. Tanto el versionamiento en URL ó el versionamiento en headers son buenas prácticas pero se ven relacionadas a como se trabajará el backend, se debe de discutir más esto último con el equipo de trabajo.
Una referencia de versionamiento en URL (Norma en API´s REST): https://medium.com/@espinozajge/versionamiento-de-apis-rest-mejores-prácticas-y-consideraciones-4b5021dd0a11
Por último por el momento, para el tema del pricing de Snowflake se ha investigado también. Snowflake utiliza un modelo de pago por consumo, lo que significa que solamente se paga por lo que se usa. Entre las consideraciones de costos están los siguientes:
Almacenamiento de datos -> Se cobra por terabyte (TB) al mes, dependiendo de la región y proveedor de nube.
Fuerza de computación -> Se paga por uso mediante créditos Snowflake. Las unidades principales de cómputo son los "virtual warehouses", agrupados por tamaño.
Funciones Serverless
Servicios en la nubes -> Snowflake coordina autenticación, seguridad y compilación de consultas.
Es importante mencionar que hay diferentes ediciones que corresponden a diferentes perfiles de clientes, los créditos en cada edición cambian en su precio por ejemplo.
Toda la información viene de la guía oficial de SF sobre su pricing, a continuación el link:
https://www.snowflake.com/wp-content/uploads/2023/12/The-Simple-Guide-to-Snowflake-Pricing.pdf
```

## SECURITY
### Prácticas de Codificación Segura
```
Qué hacer:
Implementar estándares OWASP
Aplicar principios SOLID
Seguir Clean Code practices
Implementar Twelve-Factor App methodology

### Seguridad de Datos
Qué hacer:
Diseñar cifrado de extremo a extremo (llaves de uso tiempo limitado)
Implementar gestión segura de llaves
Crear sistema de acceso por roles (RBAC)
Diseñar Row-Level Security (RLS)

Anotaciones del profesor:
Hay que asociar llaves criptograficas para cada usuario
Descifrado en memoria en el FE
ETL
Encontrar como hacer GeoAccess
Tecnologia de data transfer
IP whitelist
Casarnos con un esquema de cifrado
Sistema de logs y monitoreo de lo que esta pasando al procesar las fuentes de datos
```

## INTEGRATIONS 
### APIs y Servicios Externos
```
Qué hacer:
Definir integraciones con sistemas externos
Implementar OAuth2 y JWT
Crear esquemas de autenticación
Diseñar manejo de errores y reintentos

### Protocolos de Comunicación
Qué hacer:
Definir REST/GraphQL APIs
Implementar WebSockets para tiempo real
Crear sistemas de callbacks
Diseñar message queues
```

## QUALITY AND TESTING

### Estrategia de Pruebas
```
Qué hacer:
Definir pruebas unitarias, integración y e2e
Crear casos de prueba por componente
Implementar pruebas de seguridad
Diseñar pruebas de carga y performance
```

## DEVOPS AND DEPLOYMENT
### Gestión de Código
### CI/CD Pipeline

## MONITOREO Y OPERACIONES
### Observabilidad
```
Qué hacer:
Implementar logging centralizado
Crear métricas de aplicación
Configurar alertas y notificaciones
Diseñar dashboards operacionales
```

### Alta Disponibilidad
```
Qué hacer:
Diseñar arquitectura resiliente
Implementar load balancing
Crear estrategias de backup
Definir disaster recovery plans
```

## EVALUACIÓN Y MEJORA
### Architecture Compliance Matrix
### Análisis de Ventajas/Desventajas
```
Qué hacer:
Identificar fortalezas del diseño
Documentar limitaciones conocidas
Proponer mejoras futuras
Crear roadmap de evolución
```
### Principios de Diseño
```
Qué hacer:
Documentar principios arquitectónicos aplicados
Justificar decisiones técnicas
Crear guías de diseño para el equipo
Establecer estándares de calidad
```


