# Unit II: Knowledge Representation and Reasoning

## 1) Knowledge Representation

### What is Knowledge Representation?

**Knowledge Representation (KR)** is the process of encoding information about the world in a form that a computer system can use to solve complex tasks and draw conclusions.

**Simple Definition:**
```
Knowledge Representation = Storing information in structured format
                          so AI can understand and reason with it
```

### Why Knowledge Representation is Essential

**Problem Without KR:**
```
Raw data: "John is 25, works at Google, earns $100K"

Computer doesn't understand meaning:
- What is "25"?
- What does "Google" mean?
- Why is "$100K" important?
- Can John retire?
```

**Solution With KR:**
```
Structured representation:
  Person(John)
  Age(John, 25)
  WorksAt(John, Google)
  Salary(John, 100000)
  
Rules:
  CanRetire(X) :- Age(X, Y), Y > 65
  
Computer can now reason: Can John retire? NO (25 < 65)
```

### Goals of Knowledge Representation

A good KR system should:

1. **Represent knowledge clearly** - Easy to understand and modify
2. **Support inference** - Draw new conclusions from existing knowledge
3. **Be maintainable** - Easy to update without breaking everything
4. **Be efficient** - Reasoning happens in reasonable time
5. **Handle uncertainty** - Deal with incomplete/probabilistic information
6. **Enable sharing** - Reuse knowledge across applications

### Common Forms of Knowledge Representation

#### **1. Logical Representation**
```
Propositional Logic:
  "If it rains, the ground is wet"
  Rain → GroundWet

First-Order Logic:
  "All humans are mortal"
  ∀x (Human(x) → Mortal(x))
```

#### **2. Semantic Networks**
```
Nodes = concepts
Edges = relationships

        Parent
         / \
        /   \
     Human  Male
      / \    |
     /   \   |
  Socrates
```

#### **3. Frames**
```
Frame: Person
  Attributes:
    - Name: (string)
    - Age: (number)
    - Occupation: (string)
    - Parents: (list of persons)
```

#### **4. Rules**
```
IF condition THEN action
IF patient has fever AND cough THEN suspect flu
```

#### **5. Ontologies**
```
Structured taxonomy:
  Disease
    ├── Viral
    │   ├── Flu
    │   └── COVID
    └── Bacterial
        ├── Pneumonia
        └── Strep throat
```

### Knowledge vs. Data vs. Information

```
┌──────────────────────────────────────────────┐
│ DATA: Raw facts                              │
│ Examples: 25, John, 100000, Google           │
└──────────────────────────────────────────────┘
                    ↓
┌──────────────────────────────────────────────┐
│ INFORMATION: Organized data                  │
│ Examples: John is 25 and works at Google     │
└──────────────────────────────────────────────┘
                    ↓
┌──────────────────────────────────────────────┐
│ KNOWLEDGE: Meaningful information with rules │
│ Can answer: Can John retire? (needs rules)   │
└──────────────────────────────────────────────┘
```

---

## 2) Logical Agents

### What is a Logical Agent?

A **logical agent** is an AI agent that:
- Represents knowledge using formal logic
- Uses logical inference to derive conclusions
- Makes decisions based on logical reasoning
- Follows a knowledge and reasoning paradigm

### How a Logical Agent Works

```
┌─────────────────────────────────────────────┐
│        LOGICAL AGENT ARCHITECTURE           │
├─────────────────────────────────────────────┤
│                                             │
│  1. PERCEPTION                              │
│     ↓ Observe environment                   │
│     Percepts: "It is cloudy"                │
│                                             │
│  2. KNOWLEDGE BASE UPDATE                   │
│     ↓ Add percepts to KB                    │
│     KB: {It is cloudy, ...}                 │
│                                             │
│  3. INFERENCE                               │
│     ↓ Apply inference rules                 │
│     Query: "Will it rain?"                  │
│     Rules: Cloudy → may rain                │
│                                             │
│  4. DECISION                                │
│     ↓ Choose best action                    │
│     Action: Carry umbrella                  │
│                                             │
│  5. ACTION                                  │
│     ↓ Execute action                        │
│     Result: Carry umbrella outside          │
│                                             │
└─────────────────────────────────────────────┘
```

### Components of a Logical Agent

#### **Knowledge Base (KB)**
- Collection of sentences representing facts and rules
- All known information about the domain
- Can be queried for information

**Example:**
```
Facts:
  - Socrates is human
  - All humans are mortal
  - It is raining outside
  
Rules:
  - Human(X) → Mortal(X)
  - Raining → GroundWet
```

#### **Inference Engine**
- Uses logical rules to derive new facts
- Answers queries by reasoning
- Performs reasoning operations automatically

#### **Query Interface**
- Allows asking questions
- Returns true/false or derived facts

**Example Query:**
```
Q: Is Socrates mortal?
Inference:
  1. Socrates is human (in KB)
  2. Human(X) → Mortal(X) (in KB)
  3. Therefore, Socrates is mortal (derived)
Answer: TRUE
```

### Advantages of Logical Agents

| Advantage | Explanation |
|---|---|
| **Interpretability** | Clear why decision was made |
| **Soundness** | Conclusions are logically valid |
| **Completeness** | Can find any logical consequence |
| **Explainability** | Can show reasoning chain |
| **Flexibility** | Can update knowledge easily |

### Disadvantages of Logical Agents

| Disadvantage | Explanation |
|---|---|
| **Limited expressiveness** | Struggle with uncertainty |
| **Closed-world assumption** | Assume unknown = false |
| **Computational complexity** | Reasoning can be slow |
| **Knowledge acquisition** | Hard to encode all knowledge |
| **Scalability** | Performance degrades with KB size |

### Logical Agent Example: Medical Diagnosis

```
Knowledge Base:
  Patient(John)
  Symptom(John, fever)
  Symptom(John, cough)
  Symptom(John, fatigue)
  
  Rule1: Symptom(X, fever) ∧ Symptom(X, cough) → ProbableFlu(X)
  Rule2: ProbableFlu(X) → Prescribe(X, antivirals)

Query: What should we prescribe for John?

Inference:
  1. John has fever ✓
  2. John has cough ✓
  3. By Rule1: John probably has flu
  4. By Rule2: Prescribe antivirals for John

Answer: Prescribe antivirals for John
```

---

## 3) Propositional Logic

### What is Propositional Logic?

**Propositional Logic** is the simplest form of logic that deals with propositions (statements that are either true or false).

### Basic Concepts

#### **Proposition**
A statement that has a definite truth value:
- **True** (T or 1)
- **False** (F or 0)

**Examples of Propositions:**
```
✓ "The sky is blue" (can be true or false)
✓ "2 + 2 = 4" (true)
✓ "Paris is in Germany" (false)
✗ "What time is it?" (not a proposition - question)
✗ "x > 5" (not specific - depends on x)
```

#### **Propositional Variable**
A symbol representing a proposition:
```
P = "It is raining"
Q = "The ground is wet"
R = "I carry an umbrella"
```

### Logical Connectives (Operators)

#### **1. Negation (NOT) - ¬**
Reverses truth value

| P | ¬P |
|---|---|
| T | F |
| F | T |

**Example:**
```
P = "It is raining"
¬P = "It is not raining"

If P is true, ¬P is false
If P is false, ¬P is true
```

#### **2. Conjunction (AND) - ∧**
True only when BOTH propositions are true

| P | Q | P∧Q |
|---|---|---|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | F |

**Example:**
```
P = "It is raining"
Q = "Temperature is cold"
P∧Q = "It is raining AND temperature is cold"

Result: True only if BOTH are true
```

**Real-world use:**
```
To enter secure facility:
  Need(valid_ID ∧ passcode_correct ∧ authorized)
All three must be true
```

#### **3. Disjunction (OR) - ∨**
True when AT LEAST ONE proposition is true

| P | Q | P∨Q |
|---|---|---|
| T | T | T |
| T | F | T |
| F | T | T |
| F | F | F |

**Example:**
```
P = "You have an umbrella"
Q = "Weather is not rainy"
P∨Q = "You have an umbrella OR weather is not rainy"

Result: True if at least one is true
```

**Real-world use:**
```
To pass exam:
  Can(study_hard ∨ natural_genius ∨ luck)
At least one helps
```

#### **4. Implication (IF-THEN) - →**
"If P then Q" - False only when P is true and Q is false

| P | Q | P→Q |
|---|---|---|
| T | T | T |
| T | F | F |
| F | T | T |
| F | F | T |

**Understanding:**
```
P → Q means: "If P happens, Q must happen"

Cases:
1. P true, Q true: ✓ Promise kept
2. P true, Q false: ✗ Promise broken  
3. P false, Q true: ✓ Promise vacuously true
4. P false, Q false: ✓ Promise doesn't apply
```

**Examples:**
```
"If it rains, the ground gets wet"
"If you study, you'll pass"
"If temperature > 100°C, water boils"
```

**Tricky Case - When P is false:**
```
Statement: "If pigs can fly, I'm a millionaire"
Pigs cannot fly (P = false)
Whether I'm millionaire or not (Q = true or false)
The implication is TRUE (no promise broken)
```

#### **5. Biconditional (IF AND ONLY IF) - ↔**
True when both have SAME truth value

| P | Q | P↔Q |
|---|---|---|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | T |

**Example:**
```
P = "It is day"
Q = "Sun is up"
P ↔ Q = "It is day if and only if sun is up"

Result: True when both have same value
```

### Complex Propositional Formulas

**Example 1: Decision Tree**
```
(Hungry ∧ MoneyAvailable) ∨ (FaithfulFriend ∧ CanBorrow)
→ EatLunch

Explanation:
  Eat lunch if:
  - You're hungry AND have money, OR
  - You have faithful friend AND can borrow
```

**Example 2: Boolean Algebra**
```
(A ∨ B) ∧ (¬A ∨ C)

Truth table:
A | B | C | (A∨B) | (¬A∨C) | Result
T | T | T |   T   |   T    |   T
T | T | F |   T   |   F    |   F
T | F | T |   T   |   T    |   T
T | F | F |   T   |   F    |   F
F | T | T |   T   |   T    |   T
F | T | F |   T   |   T    |   T
F | F | T |   F   |   T    |   F
F | F | F |   F   |   T    |   F
```

### Why Propositional Logic Matters

**Advantages:**
- Simple and intuitive
- Foundation for more complex logics
- Widely used in circuit design, AI, programming
- Mechanical reasoning possible

**Limitations:**
```
Cannot express:
- Relationships: "All humans are mortal"
- Variables: "x > 5"
- Quantification: "There exists a solution"

Need First-Order Logic for these!
```

**Example of Limitation:**
```
Propositional Logic:
  S1 = "Socrates is human"
  S2 = "Plato is human"
  S3 = "Aristotle is human"
  (Repeat for each person!)
  
  Rule1: S1 → Mortal_S1
  Rule2: S2 → Mortal_S2
  Rule3: S3 → Mortal_S3
  (Repeat for each person!)

First-Order Logic (much better):
  Human(x) → Mortal(x)
  (One rule for all!)
```

---

## 4) Inference Rules

### What is Inference?

**Inference** is the process of deriving new conclusions from existing facts and rules using logical reasoning.

**Simple Concept:**
```
Known Facts + Rules → New Conclusions
```

### Key Inference Rules

#### **1. Modus Ponens (Affirming the Antecedent)**

**Form:**
```
IF P → Q (if P then Q)
AND P (P is true)
THEN Q (therefore Q is true)
```

**Explanation:**
```
The consequent Q must be true if:
- We have a rule: P implies Q
- We know P is true
```

**Example:**
```
Rule: "If it rains, the ground becomes wet"
      Rain → GroundWet

Fact: "It is raining"
      Rain (true)

Conclusion: "The ground is wet"
           GroundWet (true)
```

**Real-world example:**
```
Rule: "If you have a college degree, you can apply for the job"
Fact: "John has a college degree"
Conclusion: "John can apply for the job"
```

**Validity:**
```
✓ Modus Ponens is VALID
- If premises are true, conclusion must be true
- This is the most basic inference rule
```

---

#### **2. Modus Tollens (Denying the Consequent)**

**Form:**
```
IF P → Q (if P then Q)
AND ¬Q (Q is false)
THEN ¬P (therefore P is false)
```

**Explanation:**
```
If Q is false but the rule says P → Q,
then P cannot be true (else Q would be true)
```

**Example:**
```
Rule: "If it rains, the ground becomes wet"
      Rain → GroundWet

Fact: "The ground is NOT wet"
      ¬GroundWet (false)

Conclusion: "It is NOT raining"
           ¬Rain (false)
```

**Real-world example:**
```
Rule: "If you study hard, you will pass"
Fact: "John failed the exam"
Conclusion: "John did not study hard"
```

**Validity:**
```
✓ Modus Tollens is VALID
- Logically sound reasoning
- Frequently used in AI systems
```

---

#### **3. And Introduction (Conjunction)**

**Form:**
```
IF P (P is true)
AND Q (Q is true)
THEN P ∧ Q (therefore both P and Q are true)
```

**Example:**
```
Fact: "It is cold"     (Cold = true)
Fact: "It is rainy"    (Rainy = true)

Conclusion: "It is cold AND rainy"
           (Cold ∧ Rainy = true)
```

---

#### **4. And Elimination (Simplification)**

**Form:**
```
IF P ∧ Q (both P and Q are true)
THEN P (we can conclude P)
AND Q (we can conclude Q)
```

**Example:**
```
Fact: "It is cold AND rainy"
      (Cold ∧ Rainy = true)

Conclusions: 
  1. "It is cold" (Cold = true)
  2. "It is rainy" (Rainy = true)
```

---

#### **5. Or Introduction (Disjunction)**

**Form:**
```
IF P (P is true)
THEN P ∨ Q (we can conclude P or Q)
```

**Example:**
```
Fact: "Today is Monday"

Conclusion: "Today is Monday OR Tuesday"
(If Monday is true, the OR is automatically true)
```

---

#### **6. Or Elimination**

**Form:**
```
IF P ∨ Q (P or Q is true)
AND P → R (if P then R)
AND Q → R (if Q then R)
THEN R (therefore R must be true)
```

**Explanation:**
```
If we have P or Q, and both lead to R,
then R must be true regardless of which one is true
```

**Example:**
```
Fact: "Tomorrow is either rainy or sunny"
      (Rainy ∨ Sunny = true)

Rule1: "If rainy, bring umbrella"
       Rainy → BringUmbrella

Rule2: "If sunny, bring sunscreen"
       Sunny → BringSunscreen

Wait, this doesn't lead to single R...

Better example:
Fact: "John is either in class or library"
Rule1: "If in class, then busy"
Rule2: "If in library, then busy"
Conclusion: "John is busy" (in either case)
```

---

#### **7. Resolution**

**Form:**
```
IF (P ∨ Q) [P or Q]
AND (¬Q ∨ R) [not Q or R]
THEN (P ∨ R) [P or R]

Explanation: If Q is false, then P must be true (from first).
If Q is false, then R must be true (from second).
Therefore, P or R must be true.
```

**Example:**
```
Clause 1: "Either it's raining or road is dry"
          Rain ∨ DryRoad

Clause 2: "Road is not dry or road is slippery"
          ¬DryRoad ∨ Slippery

Resolution:
  If DryRoad is false:
    - From Clause 1: Rain must be true
    - From Clause 2: Slippery must be true
    
Conclusion: "It's raining or road is slippery"
           Rain ∨ Slippery
```

**Why Important:**
```
✓ Resolution is a complete inference method
✓ Can derive any logical consequence
✓ Basis for automatic theorem proving
✓ Used in logic programming (Prolog)
```

---

#### **8. Hypothetical Syllogism (Chain Rule)**

**Form:**
```
IF P → Q
AND Q → R
THEN P → R
```

**Example:**
```
Rule1: "If it rains, ground gets wet"
       Rain → GroundWet

Rule2: "If ground is wet, grass grows"
       GroundWet → GrassGrows

Conclusion: "If it rains, grass grows"
           Rain → GrassGrows
```

---

#### **9. Disjunctive Syllogism**

**Form:**
```
IF (P ∨ Q)
AND ¬P
THEN Q
```

**Example:**
```
Fact: "Either it's day or night"
      (Day ∨ Night)

Fact: "It's not day"
      (¬Day)

Conclusion: "It's night"
           (Night)
```

---

### Inference Process Example: Complete Reasoning

```
Knowledge Base:
  1. All students study hard
  2. All who study hard pass
  3. Raj is a student

Query: Does Raj pass?

Inference Steps:
  Step 1: Raj is a student (Fact 3)
  Step 2: All students study hard (Fact 1)
          → Raj studies hard (Modus Ponens)
  Step 3: All who study hard pass (Fact 2)
          → Raj passes (Modus Ponens)

Answer: YES, Raj passes
```

---

## 5) First-Order Logic (FOL)

### Limitations of Propositional Logic

```
Problem: Cannot express relationships and quantification

Example: "All humans are mortal"

Propositional Logic:
  H1 = "Socrates is human"
  H2 = "Plato is human"
  H3 = "Aristotle is human"
  ... (infinite propositions!)
  
  M1 = "Socrates is mortal"
  M2 = "Plato is mortal"
  M3 = "Aristotle is mortal"
  ... (infinite propositions!)

This is impractical!

First-Order Logic:
  ∀x (Human(x) → Mortal(x))
  Human(Socrates)
  ∴ Mortal(Socrates)
  
Much more elegant!
```

### What is First-Order Logic?

**First-Order Logic (FOL)** is a formal system that extends propositional logic with:
- **Objects** (individuals)
- **Predicates** (properties and relations)
- **Variables** (ranging over objects)
- **Quantifiers** (universal and existential)
- **Functions** (mapping objects to objects)

### Components of FOL

#### **1. Constants**
- Specific objects or individuals
- Written with lowercase or proper nouns
- Examples: `socrates`, `delhi`, `100`

```
Human(socrates) = "Socrates is human"
WorksAt(john, google) = "John works at Google"
```

#### **2. Variables**
- Represent generic objects
- Written with lowercase: x, y, z
- Take values from domain
- Used in quantified statements

```
∀x means "for all x"
∃x means "there exists some x"
```

#### **3. Predicates**
- Properties or relations of objects
- Written with uppercase
- Take one or more arguments

**Examples:**
```
Unary predicates (1 argument):
  Human(x) = "x is human"
  Tall(x) = "x is tall"
  
Binary predicates (2 arguments):
  Loves(x, y) = "x loves y"
  WorksAt(x, y) = "x works at y"
  
Ternary predicates (3+ arguments):
  Gave(x, y, z) = "x gave y to z"
```

#### **4. Functions**
- Map objects to objects
- Return values
- Examples: `Parent(john)`, `Plus(2, 3)`

```
MotherOf(john) = X (X is John's mother)
Age(socrates) = 70 (Socrates' age is 70)
Plus(3, 5) = 8
```

**Difference from Predicates:**
```
Predicate returns: True or False
  Human(socrates) → True

Function returns: Object or value
  MotherOf(socrates) → Xanthippe
```

#### **5. Quantifiers**

**Universal Quantifier (∀)**
- "For all"
- Means statement is true for every object in domain

**Syntax:** ∀x P(x)
**Meaning:** "For all x, P(x) is true"

**Examples:**
```
∀x (Human(x) → Mortal(x))
"For all x, if x is human, then x is mortal"
"All humans are mortal"

∀x (Student(x) → Studies(x))
"For all x, if x is a student, then x studies"
"All students study"
```

**Existential Quantifier (∃)**
- "There exists"
- Means at least one object in domain makes statement true

**Syntax:** ∃x P(x)
**Meaning:** "There exists at least one x such that P(x) is true"

**Examples:**
```
∃x (Student(x) ∧ Tall(x))
"There exists x such that x is a student and x is tall"
"There is at least one tall student"

∃x (Person(x) ∧ Lives(x, mars))
"There exists a person who lives on Mars"
```

#### **6. Logical Connectives**
Same as propositional logic: ¬, ∧, ∨, →, ↔

### FOL Formula Examples

**Example 1: Family Relationships**
```
∀x (Parent(x, y) → Older(x, y))
"For all x and y, if x is parent of y, then x is older than y"

∃x (Sibling(x, john))
"There exists someone who is John's sibling"
```

**Example 2: Properties of Numbers**
```
∀x (Number(x) ∧ Positive(x) → Greater(x, 0))
"All positive numbers are greater than zero"

∃x (Prime(x) ∧ Even(x))
"There exists a number that is both prime and even"
(The number 2!)
```

**Example 3: Domain Knowledge - Medical**
```
∀x (Patient(x) ∧ HasFever(x) ∧ HasCough(x) → MayHaveFlu(x))
"All patients with fever and cough may have flu"

∀x (HasFlu(x) → Prescribe(x, antivirals))
"All patients with flu should be prescribed antivirals"
```

### Converting English to FOL

| English | FOL |
|---|---|
| "All humans are mortal" | ∀x (Human(x) → Mortal(x)) |
| "Some students are tall" | ∃x (Student(x) ∧ Tall(x)) |
| "No student is stupid" | ¬∃x (Student(x) ∧ Stupid(x)) or ∀x (Student(x) → ¬Stupid(x)) |
| "John loves Mary" | Loves(john, mary) |
| "Everyone loves someone" | ∀x ∃y Loves(x, y) |
| "Someone is loved by everyone" | ∃y ∀x Loves(x, y) |

### Tricky Quantifier Examples

**Example 1: Order Matters**
```
∀x ∃y Loves(x, y)
"For every person x, there exists some person y that x loves"
(Everyone loves someone, possibly different people)

∃y ∀x Loves(x, y)
"There exists a person y such that every person x loves y"
(There is someone loved by everyone)

These mean DIFFERENT things!
```

**Example 2: Negation with Quantifiers**
```
¬∃x Patient(x)
"It is not the case that there exists a patient"
= "There are no patients"
= ∀x ¬Patient(x)

¬∀x Happy(x)
"It is not the case that everyone is happy"
= "Not everyone is happy"
= ∃x ¬Happy(x)

De Morgan's Laws for Quantifiers:
  ¬∃x P(x) ≡ ∀x ¬P(x)
  ¬∀x P(x) ≡ ∃x ¬P(x)
```

### Why FOL is Powerful

**Advantages:**
- ✓ Express complex relationships
- ✓ Handle quantification over domains
- ✓ Reason about properties of classes
- ✓ More expressive than propositional logic
- ✓ Foundation for most AI reasoning systems

**Limitations:**
- ✗ Cannot express everything (e.g., "most", "few")
- ✗ Reasoning can be computationally hard
- ✗ Not complete for all first-order statements

---

## 6) Inferences in First-Order Logic

### FOL Inference Rules

#### **1. Universal Instantiation**

**Concept:** If something is true for all x, it's true for any specific object.

**Form:**
```
From: ∀x P(x)
Infer: P(a)  [for any specific object a]
```

**Example:**
```
Knowledge:
  ∀x (Human(x) → Mortal(x))
  "All humans are mortal"

Specific fact:
  Human(socrates)
  "Socrates is human"

Instantiation:
  Replace x with socrates:
  Human(socrates) → Mortal(socrates)

Conclusion:
  Mortal(socrates)
  "Socrates is mortal"
```

**Practical Use:**
```
Medical Knowledge:
  ∀x (HasFlu(x) → Prescribe(x, antivirals))
  
Patient fact:
  HasFlu(john)
  
Instantiation:
  HasFlu(john) → Prescribe(john, antivirals)
  
Conclusion:
  Prescribe(john, antivirals)
```

#### **2. Existential Instantiation (Skolemization)**

**Concept:** If something exists, give it a name (Skolem constant).

**Form:**
```
From: ∃x P(x)
Infer: P(a)  [where a is a new constant not used before]
```

**Example:**
```
Knowledge:
  ∃x (Student(x) ∧ Tall(x))
  "There exists a tall student"

Existential Instantiation:
  Replace x with new constant c:
  Student(c) ∧ Tall(c)
  "Student c exists and is tall"
  
We don't know who c is, but we know such a student exists
```

**Key Point:**
```
Cannot use ∃x in reasoning - must instantiate to a constant first
The constant doesn't have to match any known object
It just represents "some object that satisfies the condition"
```

#### **3. Generalization (Universal Generalization)**

**Concept:** If P(x) is proven for arbitrary x, then ∀x P(x) is true.

**Form:**
```
From: P(x)  [proven for arbitrary x]
Infer: ∀x P(x)
```

**Caution:** Can only use if x was not previously constrained.

**Example:**
```
Proof:
  "Let x be an arbitrary student"
  "If x is a student, then x must study" (given rule)
  "Therefore x studies"
  
Generalization:
  Since x was arbitrary, we can generalize:
  "For all students x, x studies"
  ∀x (Student(x) → Studies(x))
```

#### **4. Unification**

**Concept:** Make two logical expressions identical by substituting variables.

**Simple Definition:**
```
Find variable assignments that make two expressions match
```

**Example:**
```
Expression 1: Loves(x, mother(x))
Expression 2: Loves(john, mother(john))

Unification:
  Set x = john
  Now both expressions are identical

Expression 1: Loves(john, mother(john))
Expression 2: Loves(john, mother(john))
Unification succeeded!
```

**More Complex Example:**
```
Expression 1: Parent(x, y)
Expression 2: Parent(john, mary)

Unification:
  Set x = john, y = mary
  Both become: Parent(john, mary)

Expression 1: Knows(x, x)       [someone knows themselves]
Expression 2: Knows(john, mary)

Unification:
  Try x = john and x = mary simultaneously?
  Impossible! x cannot be both john and mary
  Unification FAILS
```

**Why Important:**
```
✓ Essential for FOL reasoning
✓ Used in pattern matching
✓ Basis of Prolog programming
✓ Enables inference algorithms to work
```

#### **5. Resolution in FOL**

**Concept:** Extend propositional resolution to FOL using unification.

**General Idea:**
```
Similar to propositional resolution, but:
- Variables can be unified
- Literals can be unified before cancellation
```

**Example:**
```
Clause 1: Mortal(x) ∨ Divine(x)
Clause 2: ¬Mortal(socrates)

Resolution:
  Unify x = socrates in Clause 1:
    Mortal(socrates) ∨ Divine(socrates)
  Cancel Mortal(socrates) with ¬Mortal(socrates):
    Divine(socrates)
    
Conclusion: Socrates is divine
```

---

## 7) Ontology Engineering

### What is an Ontology?

An **ontology** is a formal, structured representation of knowledge about a domain. It specifies:
- Concepts (classes) in the domain
- Properties (attributes) of concepts
- Relationships (associations) between concepts
- Rules and constraints

**Simple Analogy:**
```
Dictionary: Defines words
Ontology: Defines concepts and how they relate

Ontology = Structured map of knowledge in a domain
```

### Components of an Ontology

#### **1. Concepts (Classes)**
- Categories of things in the domain
- Examples: Person, Disease, Medicine

```
Medical Domain Concepts:
  - Patient
  - Doctor
  - Disease
  - Medication
  - Symptom
  - Hospital
```

#### **2. Properties (Attributes)**
- Characteristics of concepts
- Have values of certain types

```
Patient properties:
  - Name (string)
  - Age (integer)
  - MedicalHistory (list)
  - BloodType (string)

Disease properties:
  - Name (string)
  - Severity (low/medium/high)
  - Infectious (boolean)
```

#### **3. Relationships (Associations)**
- How concepts relate to each other
- Often binary relations

```
Medical Domain Relationships:
  - has-symptom: Disease has-symptom Symptom
    Example: Flu has-symptom Fever
    
  - treated-by: Disease treated-by Medication
    Example: Flu treated-by Antivirals
    
  - suffers-from: Patient suffers-from Disease
    Example: John suffers-from Flu
    
  - diagnoses: Doctor diagnoses Patient
    Example: Dr.Smith diagnoses John
    
  - prescribes: Doctor prescribes Medication
    Example: Dr.Smith prescribes Antivirals
```

#### **4. Hierarchy (Taxonomy)**
- Subsumption relations
- "is-a" relationships

```
Medical Ontology Hierarchy:

        Disease
        /     \
       /       \
    Viral    Bacterial
    / \         / \
   /   \       /   \
  Flu  COVID  Pneumonia  Strep

Interpretation:
  - Flu is-a Viral disease
  - Viral is-a Disease
  - COVID is-a Viral disease
  
Inheritance:
  - Properties of Disease apply to all subclasses
  - Viral disease-specific properties apply to Flu, COVID
```

#### **5. Rules and Constraints**
- Define valid combinations
- Specify business logic

```
Rules:
  If Patient has Fever and Cough, then Patient may-have Flu
  If Patient has Flu, then Doctor should prescribe Antivirals
  
Constraints:
  - Patient Age must be positive
  - Medication dosage must be <= MaxDose
  - Only licensed Doctor can diagnose
```

### Example: Simple Medical Ontology

```
Concepts:
  Patient, Doctor, Disease, Medication, Symptom

Properties:
  Patient.name: String
  Patient.age: Integer ≥ 0
  Doctor.license_number: String
  Disease.severity: {low, medium, high}
  Medication.dose: Float

Relationships:
  has-symptom(Disease, Symptom)
  treated-by(Disease, Medication)
  suffers-from(Patient, Disease)
  diagnoses(Doctor, Patient)
  prescribes(Doctor, Medication)

Hierarchy:
  Disease
    ├── Viral
    │   ├── Flu
    │   └── COVID-19
    └── Bacterial
        ├── Pneumonia
        └── Tuberculosis

Rules:
  If suffers-from(patient, disease) and
     treated-by(disease, medicine) 
  Then can-use(patient, medicine)
```

### Ontology Engineering Process

#### **Step 1: Define Scope**
```
What domain will ontology cover?
What level of detail?
What questions should it answer?

Example:
  Domain: Medical diagnosis
  Scope: Respiratory diseases only
  Focus: Common symptoms and treatments
```

#### **Step 2: Enumerate Important Concepts**
```
List all key concepts in domain
Example for medical:
  Patient, Doctor, Disease, Symptom, Medication
  Hospital, Clinic, Test, Result
  Allergy, Side-effect, Contraindication
```

#### **Step 3: Define Concept Hierarchy**
```
Organize concepts from general to specific
Example:
  Disease (general)
    ├── RespiratoryDisease
    │   ├── CommonCold
    │   ├── Flu
    │   └── Pneumonia
    └── GastroentericDisease
        ├── Gastritis
        └── Colitis
```

#### **Step 4: Define Properties**
```
For each concept, list its properties
Example for Patient:
  - name: string
  - age: integer
  - gender: {male, female, other}
  - bloodtype: {A, B, AB, O}
  - allergies: list of Substance
```

#### **Step 5: Define Relationships**
```
How do concepts connect?
Example relationships:
  - Patient suffers-from Disease
  - Doctor treats Patient
  - Disease manifests-as Symptom
  - Medication relieves Symptom
  - Medication has-side-effect SideEffect
```

#### **Step 6: Define Rules**
```
Business rules and constraints
Example rules:
  If Patient has symptom S1 and S2,
  Then Patient may-have Disease D

  If Patient is-allergic-to Substance S,
  Then Patient cannot-use any Medication containing S
```

### Benefits of Ontologies

| Benefit | Explanation |
|---|---|
| **Knowledge Reusability** | Ontologies can be shared and reused across applications |
| **Semantic Clarity** | Clear definitions prevent ambiguity |
| **Interoperability** | Different systems can communicate using same ontology |
| **Reasoning** | Automated reasoning and inference possible |
| **Maintainability** | Centralized knowledge management |
| **Scalability** | Can expand to larger domains |

### Real-World Ontology Applications

1. **Semantic Web**
   - Web ontology language (OWL)
   - Making web content machine-readable

2. **Biomedical Informatics**
   - SNOMED-CT: Comprehensive medical terminology
   - Gene ontology for biological research

3. **E-commerce**
   - Product categories and specifications
   - Enabling semantic search

4. **Knowledge Graphs**
   - Google Knowledge Graph
   - DBpedia, Wikidata

5. **AI and Expert Systems**
   - Domain-specific knowledge representation
   - Intelligent agents

---

# Summary of Unit II

| Topic | Key Concept |
|---|---|
| **Knowledge Representation** | Encoding information for AI to understand and reason |
| **Logical Agents** | Agents using logic for reasoning and decisions |
| **Propositional Logic** | Simplest logic with true/false propositions |
| **Inference Rules** | Deriving conclusions: Modus Ponens, Resolution, etc. |
| **First-Order Logic** | More powerful logic with objects and quantifiers |
| **FOL Inference** | Universal instantiation, unification, resolution in FOL |
| **Ontology** | Structured knowledge representation of domain concepts |

---

# Exam Tips for Unit II

**2-Mark Questions:**
- Define knowledge representation
- What is propositional logic?
- Difference between ∀ and ∃
- What is unification?

**5-Mark Questions:**
- Explain FOL with examples
- How do inference rules work?
- Components of ontology
- Logical agent architecture

**10-Mark Questions:**
- Complete FOL representation of real-world domain
- Detailed inference process with multiple rules
- Ontology design for given domain
- Comparison of representation methods
