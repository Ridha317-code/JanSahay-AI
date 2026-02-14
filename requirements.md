# Requirements Document

## Introduction

JanSahay AI is an AI-powered Government Scheme Assistant that helps citizens discover and understand government schemes they are eligible for. The system accepts natural language queries, extracts demographic information, matches users to relevant schemes, and provides clear explanations in multiple languages while ensuring data privacy and responsible AI practices.

## AI Necessity and Justification

### Why AI is Essential

Traditional rule-based systems are insufficient for JanSahay AI due to the inherent complexity and variability of citizen interactions with government schemes:

**1. Natural Language Understanding Complexity:**
- Citizens describe their situations in diverse, unstructured ways using colloquial language, regional dialects, and varying levels of literacy
- Rule-based keyword matching fails to capture intent when users phrase queries differently: "I am old and poor" vs "Senior citizen with low income" vs "Retired person needing financial help"
- LLMs are necessary to understand semantic meaning, handle paraphrasing, and extract intent across linguistic variations

**2. Contextual and Probabilistic Reasoning:**
- Eligibility is not always binary; many schemes have soft criteria, exceptions, and contextual factors
- Users provide incomplete information incrementally across conversation turns
- AI-driven probabilistic inference is needed to:
  - Reason about partial information with confidence scoring
  - Aggregate context across multiple messages
  - Rank schemes by relevance, not just binary eligibility
  - Handle uncertainty transparently

**3. Demographic Extraction from Unstructured Text:**
- Users don't fill forms; they describe their lives naturally: "I'm 65, live in rural Maharashtra, my pension is small"
- Neural NER models are essential to:
  - Extract multiple demographic entities from free-form text
  - Handle Indian-specific entities (states, caste categories, income ranges)
  - Assign confidence scores to uncertain extractions
  - Resolve entities across conversation turns

**4. Multilingual Intelligence:**
- India has 22 official languages and hundreds of dialects
- Simple word-for-word translation fails for government terminology and legal requirements
- Neural machine translation is necessary to:
  - Preserve semantic accuracy of eligibility criteria across languages
  - Handle code-switching and regional variations
  - Maintain consistency in legal and procedural terminology

**5. Personalized Explanation Generation:**
- Government scheme documents are written in bureaucratic language inaccessible to most citizens
- Each user needs explanations tailored to their specific situation
- LLM-based summarization is essential to:
  - Generate simple, grade-8 reading level explanations
  - Highlight benefits relevant to each user's demographics
  - Explain eligibility reasoning in natural language
  - Adapt tone and detail level to user needs

**6. Fairness and Bias Mitigation:**
- Manual rule systems can encode human biases in eligibility logic
- AI enables:
  - Continuous monitoring for discriminatory patterns
  - Counterfactual fairness testing
  - Transparent, auditable decision-making
  - Adaptive learning from fairness feedback

**Conclusion:** JanSahay AI requires sophisticated AI capabilities—LLMs for understanding and generation, neural NER for extraction, probabilistic models for reasoning, and neural MT for multilingual support—to deliver accessible, fair, and intelligent government scheme assistance at scale.

## AI Model Considerations

### Large Language Models (LLMs)

**Purpose:** Intent classification, dialogue management, and summarization

**Requirements:**
- Fine-tuned transformer models (e.g., GPT-based, BERT-based, or Llama-based) for Indian context
- Support for multilingual understanding (Hindi, English, regional languages)
- Prompt engineering for consistent, accurate responses
- Context window sufficient for multi-turn conversations (8K+ tokens)

**Deployment:**
- Cloud-hosted inference endpoints (AWS SageMaker, Bedrock, or Azure OpenAI)
- Latency target: <1 second for intent classification, <2 seconds for summarization
- Fallback mechanisms for service unavailability

### Named Entity Recognition (NER) Models

**Purpose:** Demographic extraction from unstructured text

**Requirements:**
- Custom-trained NER models for Indian demographic entities:
  - Age, income, occupation, location (state/district)
  - Caste category, disability type, education level, family composition
- Confidence scoring for each extracted entity
- Support for multilingual input (Hindi, English, regional languages)

**Training Data:**
- Annotated corpus of Indian demographic descriptions
- Diverse linguistic variations and regional phrasings
- Edge cases: ambiguous ages, income ranges, location variations

**Performance Targets:**
- Entity extraction accuracy: >85% F1 score
- Confidence calibration: predicted confidence correlates with actual accuracy

### Probabilistic Ranking Models

**Purpose:** Relevance-based scheme recommendation

**Requirements:**
- ML models to score scheme relevance beyond binary eligibility
- Features: demographic match, expressed needs, benefit magnitude, application complexity
- Ranking algorithm that balances exact matches with near-matches

**Approach:**
- Hybrid: rule-based eligibility filtering + ML-based relevance scoring
- Learning-to-rank models trained on user engagement data (anonymized)
- Continuous refinement based on user feedback

**Performance Targets:**
- Matching precision: >90% (recommended schemes are truly eligible)
- Matching recall: >80% (most eligible schemes are recommended)
- Ranking quality: relevant schemes in top 3 positions >85% of the time

### Neural Machine Translation (NMT) Models

**Purpose:** Multilingual support with semantic preservation

**Requirements:**
- State-of-the-art NMT models for 7+ Indian languages
- Fine-tuning on government domain terminology
- Bidirectional translation: regional languages ↔ English (internal format)

**Quality Assurance:**
- Back-translation verification for critical information (eligibility criteria, requirements)
- Human evaluation of translation quality for government terminology
- Validation that numeric values (ages, income) are preserved accurately

**Performance Targets:**
- Translation accuracy: BLEU score >40 for government domain
- Latency: <500ms per translation
- Semantic preservation: >95% accuracy for eligibility criteria

### Bias Evaluation and Fairness Mechanisms

**Purpose:** Ensure equitable recommendations across demographic groups

**Requirements:**
- Automated fairness audits comparing recommendation distributions
- Statistical parity checks for protected attributes (caste, gender, religion, disability)
- Counterfactual fairness testing: profiles differing only in protected attributes receive equivalent recommendations

**Monitoring:**
- Continuous logging of matching decisions (anonymized)
- Anomaly detection for unexpected disparities
- Human-in-the-loop review for flagged cases

**Governance:**
- Regular model retraining with diverse, representative data
- Adversarial testing to identify bias edge cases
- Ethical AI guidelines enforced throughout development

## Success Metrics

### AI Performance Metrics

**Entity Extraction Accuracy:**
- Target: >85% F1 score for demographic entity extraction
- Measured on held-out test set with diverse linguistic variations
- Per-entity accuracy tracked: age, income, location, occupation, etc.

**Intent Classification Accuracy:**
- Target: >90% accuracy for intent classification
- Measured across intent types: query_schemes, provide_info, ask_details, modify_info
- Confusion matrix analysis to identify misclassification patterns

**Matching Precision and Recall:**
- Precision target: >90% (recommended schemes are truly eligible)
- Recall target: >80% (most eligible schemes are recommended)
- Measured against ground truth eligibility labels

**Ranking Quality:**
- Target: Most relevant scheme in top 3 positions >85% of the time
- Measured using Normalized Discounted Cumulative Gain (NDCG)
- User engagement signals (clicks, detail views) as relevance indicators

**Translation Quality:**
- Target: BLEU score >40 for government domain translations
- Human evaluation: >4/5 rating for fluency and accuracy
- Semantic preservation: >95% accuracy for eligibility criteria

### System Performance Metrics

**Response Latency:**
- Target: <2 seconds end-to-end response time (95th percentile)
- Intent classification: <1 second
- Scheme matching: <500ms
- Summarization: <1.5 seconds
- Translation: <500ms

**Throughput:**
- Target: Support 10,000 concurrent sessions
- Handle 1 million daily active users
- Auto-scaling to handle 10x traffic spikes

**Availability:**
- Target: 99.9% uptime (less than 43 minutes downtime per month)
- Graceful degradation during partial outages
- Fallback mechanisms for AI service failures

### User Experience Metrics

**User Satisfaction:**
- Target: >4.0/5.0 average satisfaction rating
- Measured via post-interaction surveys
- Net Promoter Score (NPS): >50

**Task Completion Rate:**
- Target: >80% of users successfully find relevant schemes
- Measured by tracking users who view scheme details after matching
- Abandonment rate: <20%

**Conversation Efficiency:**
- Target: Average 3-5 message exchanges to complete scheme discovery
- Minimize unnecessary clarification requests
- Reduce user frustration from repetitive questions

**Accessibility:**
- Target: >70% of interactions in non-English languages
- Support for users with varying literacy levels
- Screen reader compatibility for web interface

### Fairness and Responsible AI Metrics

**Demographic Parity:**
- Target: Recommendation rates differ by <5% across protected groups (caste, gender, religion, disability)
- Measured via statistical parity analysis
- Regular audits to detect emerging disparities

**Explainability:**
- Target: 100% of recommendations include human-readable explanations
- User comprehension: >80% of users understand why schemes were recommended

**Privacy Compliance:**
- Target: 100% of sessions comply with data retention policies
- Zero PII leaks in logs or analytics
- User consent obtained before data collection

**Bias Incident Rate:**
- Target: <1% of interactions flagged for potential bias
- All flagged cases reviewed within 24 hours
- Continuous model improvement based on bias reports

### Business Impact Metrics

**Scheme Awareness:**
- Target: Increase citizen awareness of eligible schemes by 50%
- Measured via surveys and scheme application rates

**Application Conversion:**
- Target: >30% of users who find schemes proceed to apply
- Track referrals to application portals

**Cost Efficiency:**
- Target: <₹1 per user interaction (infrastructure + AI costs)
- Optimize through caching, batching, and efficient model serving

**Scalability:**
- Target: Support 100 million users nationwide
- Linear cost scaling with user growth



## Glossary

- **JanSahay_AI**: The AI-powered Government Scheme Assistant system
- **User**: A citizen seeking information about government schemes
- **Scheme**: A government program or benefit offering
- **Demographic_Data**: User information including age, income, location, occupation, and other relevant attributes
- **NLP_Engine**: Natural Language Processing component that interprets user queries
- **Scheme_Matcher**: Component that matches user demographics to eligible schemes
- **Summarizer**: Component that generates simple language summaries of schemes
- **Language_Processor**: Component that handles multilingual input and output
- **Privacy_Manager**: Component that ensures data privacy and compliance

## Requirements

### Requirement 1: Natural Language Input Processing

**User Story:** As a user, I want to describe my situation in natural language, so that I can easily inquire about schemes without filling complex forms.

#### Acceptance Criteria

1. WHEN a user submits a text query in natural language, THE NLP_Engine SHALL parse and understand the query intent
2. WHEN a query contains demographic information, THE NLP_Engine SHALL extract relevant attributes (age, income, location, occupation, family size, disability status, education level)
3. WHEN a query is ambiguous or incomplete, THE JanSahay_AI SHALL identify missing information and prompt the user for clarification
4. WHEN a query contains multiple intents, THE NLP_Engine SHALL identify all intents and process them appropriately
5. THE NLP_Engine SHALL handle colloquial language, abbreviations, and common misspellings

### Requirement 2: Demographic Data Extraction

**User Story:** As a user, I want the system to understand my demographic details from my description, so that I don't have to fill out structured forms.

#### Acceptance Criteria

1. WHEN demographic information is mentioned in user input, THE JanSahay_AI SHALL use neural NER models to extract and structure the following attributes: age, gender, income level, location (state/district), occupation, family composition, disability status, caste/category, education level
2. WHEN extracted demographic data is uncertain, THE JanSahay_AI SHALL use ML-based confidence scoring to assign probabilistic confidence scores to each extracted attribute
3. WHEN confidence scores are below a threshold, THE JanSahay_AI SHALL request confirmation from the user before proceeding with uncertain information
4. THE JanSahay_AI SHALL normalize extracted data into standardized formats using intelligent parsing (e.g., income ranges, location codes)
5. WHEN users provide partial information across multiple messages, THE JanSahay_AI SHALL use contextual aggregation to maintain context and build a complete demographic profile incrementally

### Requirement 3: Scheme Matching

**User Story:** As a user, I want to receive a list of government schemes I'm eligible for, so that I can benefit from relevant programs.

#### Acceptance Criteria

1. WHEN user demographic data is available, THE Scheme_Matcher SHALL use AI-driven probabilistic reasoning to identify all schemes where the user meets eligibility criteria
2. WHEN multiple schemes are matched, THE Scheme_Matcher SHALL rank them by relevance using ML-based scoring that considers demographic fit, expressed needs, and benefit magnitude
3. WHEN no schemes match exactly, THE Scheme_Matcher SHALL use contextual inference to identify near-match schemes and explain what criteria are not met
4. THE Scheme_Matcher SHALL consider all eligibility dimensions including age, income, location, occupation, gender, disability, education, and family composition through comprehensive feature analysis
5. WHEN scheme eligibility rules change, THE Scheme_Matcher SHALL use the current active rules for matching

### Requirement 4: Scheme Summarization

**User Story:** As a user, I want scheme details explained in simple language, so that I can easily understand benefits and application processes.

#### Acceptance Criteria

1. WHEN a scheme is presented to a user, THE Summarizer SHALL use LLM-based generation to create a summary including: scheme name, key benefits, eligibility criteria, required documents, and application process
2. THE Summarizer SHALL use AI-driven text simplification to produce output at a reading level appropriate for general audiences (approximately 8th grade level)
3. THE Summarizer SHALL avoid technical jargon and government terminology through intelligent paraphrasing, or explain such terms when necessary
4. WHEN scheme details are lengthy, THE Summarizer SHALL provide a brief overview with option to view full details
5. THE Summarizer SHALL use contextual reasoning to highlight the most relevant benefits based on user's specific situation

### Requirement 5: Multilingual Support

**User Story:** As a user, I want to interact with the system in my preferred language, so that I can understand information in my native language.

#### Acceptance Criteria

1. THE Language_Processor SHALL use neural machine translation to support input and output in at least Hindi, English, and 5 major regional languages (Tamil, Telugu, Bengali, Marathi, Gujarati)
2. WHEN a user submits input in a supported language, THE Language_Processor SHALL use neural language detection models to identify the language automatically
3. WHEN a user's language is detected, THE JanSahay_AI SHALL respond in the same language using NMT
4. WHERE a user explicitly requests a language change, THE JanSahay_AI SHALL switch to the requested language for all subsequent interactions
5. WHEN translating scheme information, THE Language_Processor SHALL use semantic preservation techniques to maintain accuracy of eligibility criteria and application requirements

### Requirement 6: Data Privacy and Security

**User Story:** As a user, I want my personal information to be protected, so that my privacy is maintained and data is not misused.

#### Acceptance Criteria

1. THE Privacy_Manager SHALL not store personally identifiable information (PII) beyond the active session unless explicitly consented by the user
2. WHEN a session ends, THE Privacy_Manager SHALL delete all user demographic data and conversation history
3. THE JanSahay_AI SHALL encrypt all data in transit using TLS 1.3 or higher
4. THE JanSahay_AI SHALL not share user data with third parties without explicit user consent
5. WHEN processing user data, THE Privacy_Manager SHALL anonymize data used for analytics and system improvement
6. THE JanSahay_AI SHALL provide users with a clear privacy notice before collecting any information

### Requirement 7: Responsible AI Practices

**User Story:** As a system administrator, I want the AI to operate fairly and transparently, so that all users receive equitable service.

#### Acceptance Criteria

1. THE JanSahay_AI SHALL not discriminate based on protected attributes (caste, religion, gender, disability) when matching schemes
2. WHEN providing scheme recommendations, THE JanSahay_AI SHALL explain the reasoning behind matches in understandable terms
3. THE JanSahay_AI SHALL provide users with a way to report incorrect or biased responses
4. WHEN the AI is uncertain about a response, THE JanSahay_AI SHALL communicate uncertainty rather than providing potentially incorrect information
5. THE JanSahay_AI SHALL maintain audit logs of matching decisions for fairness monitoring (without storing PII)

### Requirement 8: Conversation Management

**User Story:** As a user, I want to have a natural conversation with the system, so that I can ask follow-up questions and refine my search.

#### Acceptance Criteria

1. WHEN a user asks a follow-up question, THE JanSahay_AI SHALL maintain conversation context from previous messages in the session
2. WHEN a user requests more details about a specific scheme, THE JanSahay_AI SHALL provide comprehensive information about that scheme
3. WHEN a user wants to modify their demographic information, THE JanSahay_AI SHALL update the profile and re-match schemes
4. THE JanSahay_AI SHALL support common conversation patterns including greetings, clarifications, and corrections
5. WHEN a conversation becomes too long or context is lost, THE JanSahay_AI SHALL offer to start fresh with a summary of current understanding

### Requirement 9: Error Handling and Fallback

**User Story:** As a user, I want helpful responses even when the system doesn't understand me, so that I can still get assistance.

#### Acceptance Criteria

1. WHEN the NLP_Engine cannot understand a query, THE JanSahay_AI SHALL ask clarifying questions rather than failing silently
2. WHEN no schemes match a user's profile, THE JanSahay_AI SHALL provide helpful guidance on alternative resources or nearby eligibility
3. IF the system encounters a technical error, THEN THE JanSahay_AI SHALL provide a user-friendly error message and suggest retry or alternative actions
4. WHEN a requested language is not supported, THE JanSahay_AI SHALL inform the user and offer available language options
5. THE JanSahay_AI SHALL provide a way for users to connect with human support when automated assistance is insufficient

### Requirement 10: Scheme Database Management

**User Story:** As a system administrator, I want to maintain an up-to-date scheme database, so that users receive accurate and current information.

#### Acceptance Criteria

1. THE JanSahay_AI SHALL maintain a structured database of government schemes with eligibility rules, benefits, and application procedures
2. WHEN scheme information is updated, THE JanSahay_AI SHALL reflect changes immediately in matching and summarization
3. THE JanSahay_AI SHALL track scheme metadata including effective dates, expiration dates, and geographic applicability
4. THE JanSahay_AI SHALL support schemes at central, state, and district levels
5. WHEN a scheme is discontinued, THE JanSahay_AI SHALL mark it as inactive and not recommend it to users
