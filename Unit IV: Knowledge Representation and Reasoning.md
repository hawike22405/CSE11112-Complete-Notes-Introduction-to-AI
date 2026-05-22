# Unit IV: Knowledge Representation and Reasoning

## KNOWLEDGE REPRESENTATION OVERVIEW

Knowledge representation (KR) is the study of how to formally represent knowledge in a way that enables computers to reason about it effectively.

### Key Components
- **Declarative Knowledge**: Facts and relationships (what we know)
- **Procedural Knowledge**: How to perform tasks (how to do things)
- **Meta-knowledge**: Knowledge about knowledge (knowing about knowing)

### Requirements for Good KR System
- Expressiveness: Capture complex information
- Efficiency: Reasoning should be computationally tractable
- Interpretability: Humans should understand representations
- Modularity: Easy to add/modify knowledge

---

## 1. LOGIC-BASED REPRESENTATION

### Propositional Logic
- Deals with propositions (true/false statements)
- Logical operators: AND (∧), OR (∨), NOT (¬), IMPLIES (→), IFF (↔)
- Example: "If it is raining, then the ground is wet"
  - P: "It is raining"
  - Q: "The ground is wet"
  - Expression: P → Q

**Advantages**:
- Simple and well-understood
- Effective theorem proving
- Decidable (can determine truth/falsehood)

**Disadvantages**:
- Cannot represent relations between objects
- Cannot express universal statements
- Limited expressiveness

### First-Order Logic (FOL)
- Extends propositional logic with predicates, variables, and quantifiers
- Components:
  - **Constants**: Objects (john, mary, red)
  - **Variables**: Placeholders (x, y, z)
  - **Predicates**: Relations (likes(X, Y), mortal(X))
  - **Quantifiers**: ∀ (for all), ∃ (exists)

**Examples**:
∀x (Human(x) → Mortal(x)) "All humans are mortal"

∃x (Bird(x) ∧ CanFly(x)) "There exists a bird that can fly"

∀x ∀y (Parent(x, y) → Ancestor(x, y)) "All parents are ancestors"

Code

**Inference Rules**:
- **Modus Ponens**: If P and P→Q, then Q
- **Modus Tollens**: If ¬Q and P→Q, then ¬P
- **Universal Instantiation**: From ∀x P(x), derive P(a)
- **Generalization**: From P(a), derive ∀x P(x) (if a is arbitrary)

**Normal Forms**:
- **Conjunctive Normal Form (CNF)**: AND of ORs
  - (A ∨ B) ∧ (¬B ∨ C) ∧ (¬A ∨ C)
- **Disjunctive Normal Form (DNF)**: OR of ANDs
  - (A ∧ B) ∨ (¬B ∧ C) ∨ (¬A ∧ C)

**Resolution**:
- Proof by contradiction technique
- Convert to CNF, negate conclusion, derive empty clause
- Sound and complete for FOL

**Advantages**:
- Highly expressive
- Can represent complex relationships
- Well-established inference mechanisms

**Disadvantages**:
- Computationally expensive (NP-hard)
- Difficult to handle uncertainty
- Rigid rules don't handle exceptions well

---

## 2. SEMANTIC NETWORKS

### Structure
- Nodes represent objects/concepts
- Labeled edges represent relationships
- Visual representation of knowledge

### Components
Code
[Dog]
  |
instance
  |
[Fido]

[Dog]----subclass----[Animal]
[Dog]----hasProperty----[fourLegs]
Code

### Properties
- **Inheritance**: Properties inherited through subclass/instance relationships
- **Semantic Distance**: Closer nodes are more related
- **Transitivity**: Some relationships inherit through chains

**Example Network**:
[Canary] |----instance----[Tweety] |----subclass----[Bird] |----color----[yellow]

[Bird] |----subclass----[Animal] |----hasFeature----[wings] |----canFly----[yes]

[Animal] |----hasProperty----[hasLegs]

Code

**Issues**:
- No standardized semantics
- Ambiguous relationship interpretation
- Non-monotonic (adding facts can remove conclusions)

**Advantages**:
- Intuitive and visual
- Efficient retrieval through spreading activation
- Easy for humans to understand

**Disadvantages**:
- Difficult formal reasoning
- Ambiguous semantics
- Limited inference capabilities

---

## 3. FRAMES AND SCRIPTS

### Frames
- Data structure containing slots for attributes
- Each slot has facets (properties)
- Provides default values and methods

**Frame Structure**:
Frame: BIRD Slot: Wings Facet: Value = two Facet: Default = two Facet: Range = 0 to 3

Slot: CanFly Facet: Value = yes Facet: Default = yes Facet: Exception = penguin

Slot: Diet Facet: Value = seeds Facet: Method = inherited_from(ANIMAL)

Code

### Frame Inheritance
[ANIMAL] legs: 4 eyes: 2 | V [BIRD] wings: 2 canFly: yes | V [PENGUIN] wings: 2 canFly: no (override) habitat: ice

Code

### Scripts
- Represent temporal sequences of events
- Similar to frames but for events/situations
- Contains scenes (subscripts)

**Example - Restaurant Script**:
Script: RESTAURANT_VISIT

Preconditions:

Actor is hungry
Actor has money
Props: Tables, chairs, food, money

Scenes:

ENTERING: Actor enters, finds table, sits
ORDERING: Waiter brings menu, actor orders
EATING: Food arrives, actor eats
PAYING: Actor pays bill, tips waiter
LEAVING: Actor leaves
Results:

Actor is full
Actor has less money
Restaurant has more money
Code

**Advantages**:
- Efficient for typical situations
- Flexible with defaults and overrides
- Good for common-sense reasoning

**Disadvantages**:
- Hard to represent exceptions
- Not suitable for logical reasoning
- Semantic ambiguity

---

## 4. PRODUCTION RULES AND EXPERT SYSTEMS

### Production Rules
- IF-THEN statements representing domain knowledge
- Format: IF (conditions) THEN (actions/conclusions)

**Examples**:
Rule 1: IF (temperature > 100°C) THEN (boil) Rule 2: IF (humidity > 80%) AND (temperature > 25°C) THEN (uncomfortable) Rule 3: IF (animal has feathers) AND (animal can fly) THEN (bird)

Code

### Expert System Architecture
┌─────────────────┐ │ USER INTERFACE│ └────────┬────────┘ │ ┌────────▼────────────┐ │ INFERENCE ENGINE │ │ (Forward/Backward) │ └────────┬────────────┘ │ ┌────┴────┐ │ │ ┌───▼──┐ ┌───▼──────┐ │ KB │ │WORKING │ │(Rules) │MEMORY │ └──────┘ │(Facts) │ └──────┘

Code

### Inference Strategies

**Forward Chaining**:
- Start with known facts
- Apply rules to derive new facts
- Continue until goal is reached or no new facts
- "Data-driven" reasoning
- Used for monitoring, planning

Known: temperature = 101°C Rule: IF temperature > 100°C THEN boil = true Result: boil = true

Code

**Backward Chaining**:
- Start with goal
- Find rules that prove goal
- Recursively prove rule preconditions
- "Goal-driven" reasoning
- Used for diagnostic systems, question answering

Goal: Is it boiling? Find rule: IF temperature > 100°C THEN boil Subgoal: Is temperature > 100°C? Check facts: temperature = 101°C ✓ Conclusion: Yes, it's boiling

Code

### Conflict Resolution
When multiple rules can fire simultaneously:
1. **Specificity**: Choose most specific rule
2. **Recency**: Choose most recently used fact
3. **Priority**: Rules have explicit priorities
4. **LIFO**: Last In, First Out order
5. **LEX**: Lexicographic ordering

### MYCIN - Classic Expert System
- Medical diagnosis for bacterial infections
- 600+ production rules
- Confidence factors (0-1) for uncertainty
- Backward chaining with hypothesis management

---

## 5. DESCRIPTION LOGICS

### Overview
- Formal system balancing expressiveness and decidability
- Subset of FOL designed for efficient reasoning
- Used in ontologies (OWL, RDF)

### Syntax
- **Concepts**: Classes (Bird, Penguin)
- **Roles**: Relations (flies, hasWing)
- **Individuals**: Instances (tweety)

**Example**:
Penguin ⊆ Bird ⊓ ¬CanFly ⊓ ∃livesIn.IcyRegion "Penguins are birds that cannot fly and live in icy regions"

Bird ≡ Animal ⊓ ∃hasWing.Wing "Birds are animals that have wings"

Code

### Reasoning Tasks
- **Classification**: Is instance member of class?
- **Consistency Checking**: Can facts coexist?
- **Subsumption**: Is one concept subsumed by another?
- **Retrieval**: Find all individuals of class

**Advantages**:
- Expressive yet computationally tractable
- Formal semantics
- Automated reasoning guaranteed

**Disadvantages**:
- Less expressive than FOL
- Limited ability to handle rules
- Requires translation to/from FOL

---

## 6. ONTOLOGIES

### Definition
- Formal specification of conceptualization
- Defines concepts, relationships, and constraints in domain
- Enables knowledge sharing and reuse

### Components
Classes (Concepts):

Animal
Mammal
Dog
Cat
Bird
Penguin
Properties (Attributes):

hasColor: hasValue → Color
hasAge: hasValue → Integer
canFly: hasValue → Boolean
Relations:

isPartOf
hasFeature
canEat
Code

### Ontology Languages
- **RDF (Resource Description Framework)**: Subject-Predicate-Object triples
- **OWL (Web Ontology Language)**: Based on description logics
- **SKOS (Simple Knowledge Organization System)**: For thesauri

### Example - Animal Ontology
Class Animal properties: - hasLegs: Integer - hasColor: Color - hasHabitat: String

Class Mammal subClassOf Animal properties: - hasFur: Boolean - produceMilk: Boolean

Class Dog subClassOf Mammal properties: - canBark: Boolean = true - domesticated: Boolean = true

Individual Fido instanceOf Dog properties: - hasColor: brown - hasName: "Fido"

Code

**Applications**:
- Semantic web
- Knowledge management systems
- Medical informatics (SNOMED CT, ICD)
- Enterprise knowledge bases

---

## 7. UNCERTAINTY AND NON-MONOTONIC REASONING

### Problems with Classical Logic
- Real-world knowledge is uncertain
- New information can invalidate old conclusions
- Common-sense reasoning requires defaults

### Non-Monotonic Logic
- **Monotonic**: Once proven true, always true
- **Non-Monotonic**: New facts can make conclusions false

**Example**:
Monotonic: Penguin is a bird (true) Birds can fly (true) Therefore: Penguins can fly (true) ✗ WRONG!

Non-Monotonic: Birds can fly (default) Penguins are birds (true) Penguins cannot fly (exception) Conclusion: Penguins cannot fly ✓

Code

### Default Logic
- Provide default rules and exceptions
- Normal forms: "Typically X is Y"

Default Rules: Bird(x) : CanFly(x) / CanFly(x) "If x is a bird, assume x can fly"

Penguin(x) : ¬CanFly(x) / ¬CanFly(x) "If x is a penguin, assume x cannot fly"

Facts: Penguin(tweety) → ¬CanFly(tweety) (more specific)

Code

### Circumscription
- Minimize extension of predicates
- Assume false anything not proven true
- "Closed World Assumption"

### Belief Revision
- Incorporate new information
- Maintain consistency
- Minimal change principle

---

## 8. REASONING WITH UNCERTAINTY

### Probability Theory
- Assign probabilities to propositions
- P(Event) ∈ [0, 1]
- Bayes' theorem: P(A|B) = P(B|A) × P(A) / P(B)

### Certainty Factors (MYCIN Style)
- CF ∈ [-1, 1]
- CF = 1: Absolutely true
- CF = 0: Unknown
- CF = -1: Absolutely false

If disease X and symptom Y observed: CF(diagnosis) = 0.8 Confidence in conclusion is 80%

Code

### Fuzzy Logic
- Truth values between 0 and 1
- Handles vague concepts (tall, warm, near)

Membership function for "Tall": Height < 5'6": μ(Tall) = 0.0 Height 5'6" - 6': μ(Tall) = 0.5 to 0.8 Height > 6': μ(Tall) = 1.0

Code

---

## COMPARISON OF REPRESENTATIONS

| Feature | Logic | Semantic Nets | Frames | Rules | Description Logic |
|---------|-------|---------------|--------|-------|------------------|
| Expressiveness | Very High | Medium | High | High | Medium-High |
| Reasoning Power | Strong | Weak | Moderate | Strong | Strong |
| Efficiency | Low | High | High | High | Medium |
| Human Readable | Moderate | High | High | Very High | Moderate |
| Handles Uncertainty | Difficult | No | No | Yes (CF) | No |
| Inheritance | No | Yes | Yes | No | Yes |

---

## KEY CONCEPTS

### Representation Tradeoffs
- **Expressiveness vs. Tractability**: More expressive = harder to compute
- **Generality vs. Efficiency**: General systems are slower
- **Clarity vs. Completeness**: Simple representations miss nuances

### Reasoning Types
1. **Deductive**: Derive specific from general (valid/invalid)
2. **Inductive**: Derive general from specific (probable)
3. **Abductive**: Infer explanation for observation (diagnostic)
4. **Analogical**: Transfer knowledge from similar situation

### Common-Sense Reasoning
- Use background knowledge and defaults
- Handle incomplete information
- Understand typical situations
- Learn from experience

---

## EXAM PREPARATION TIPS

- **Know tradeoffs**: Each representation has strengths/weaknesses
- **Understand inference**: How reasoning works in each paradigm
- **Compare systems**: Know when to use which approach
- **Practical application**: Map real problems to representations
- **Hybrid approaches**: Combine multiple representations for robustness

---

**Complete unit covering all knowledge representation paradigms, reasoning mechanisms, and practical applications for Introduction to AI.**
This comprehensive Unit IV content covers:

Logic-based representations (Propositional, First-Order Logic)
Semantic networks
Frames and scripts
Production rules and expert systems
Description logics
Ontologies
Non-monotonic reasoning
Uncertainty handling
Comparative analysis and exam tips
