# Unit III: Planning and Uncertainty

## 1) Planning with State-Space Search

### What is Planning?

**Planning** is the process of finding a sequence of actions that transforms the current state into a goal state.

**Simple Definition:**
```
Planning = Finding a sequence of actions to achieve a goal
```

### State-Space Representation

A state-space is a formal way to represent a planning problem:

```
Components:
  1. Initial State: Starting situation
  2. Goal State: Desired situation
  3. Actions: Possible moves/operations
  4. Transition Model: What happens after each action
  5. Path Cost: Cost of executing actions
  6. State Space: All possible states reachable
```

### Planning as Search

Planning can be solved using search algorithms:

```
Problem Formulation:
  - States: Different configurations of world
  - Actions: Change state from one to another
  - Search: Find path from initial to goal state
  
Algorithm:
  1. Start from initial state
  2. Apply possible actions
  3. Generate new states
  4. Continue until goal state reached
  5. Return sequence of actions
```

### Example 1: Robot Navigation Planning

```
Domain: Robot moving in building

Initial State:
  Robot at Room A
  Door1 is locked
  Key is in Room A

Goal State:
  Robot in Room C

Actions:
  1. Move(from_room, to_room) - move between connected rooms
  2. Unlock(door) - unlock a door
  3. PickUp(object) - pick object
  4. Drop(object) - drop object

Plan:
  1. PickUp(key)            [Take key from Room A]
  2. Move(RoomA, RoomB)     [Move to Room B]
  3. Move(RoomB, DoorWay)   [Approach Door1]
  4. Unlock(Door1)          [Unlock with key]
  5. Move(DoorWay, RoomC)   [Enter Room C]
  
Goal reached!
```

### Example 2: Project Planning

```
Goal: Complete software project

Initial State:
  - Requirements NOT analyzed
  - Design NOT done
  - Code NOT written
  - Testing NOT done
  
Goal State:
  - Project delivered
  
Actions:
  1. AnalyzeRequirements (duration: 1 week)
  2. Design (depends on: AnalyzeRequirements)
  3. Code (depends on: Design)
  4. Test (depends on: Code)
  5. Deploy (depends on: Test)

Plan (with dependencies):
  Week 1: AnalyzeRequirements
  Week 2: Design (after requirements)
  Weeks 3-5: Code (after design)
  Week 6: Test (after coding)
  Week 7: Deploy (after testing)
```

### State-Space Search Approaches

#### **1. Forward Search (Progression)**

**Concept:** Start from initial state, apply actions, move toward goal.

```
Initial State → Action1 → State1 → Action2 → State2 → ... → Goal

Process:
  1. Start at initial state
  2. Apply all possible actions
  3. Generate successor states
  4. Continue until goal reached
  5. Return action sequence
```

**Advantages:**
- Intuitive and natural
- Can detect unsolvable problems early
- Well-suited when initial state is known

**Disadvantages:**
- May explore many irrelevant states
- Branching factor can be huge
- Inefficient without good heuristics

**Example:**
```
Goal: Reach Room C

Initial: RoomA

Forward Search Tree:
                    RoomA
                   /     \
              RoomB       RoomD
             /    \       /    \
         RoomA  RoomC  RoomC  RoomE
         (loop)  ✓Goal

Conclusion: Found path RoomA → RoomB → RoomC
```

#### **2. Backward Search (Regression)**

**Concept:** Start from goal state, work backward to initial state.

```
Goal ← Action^-1 ← Previous State ← ... ← Initial State

Process:
  1. Start at goal state
  2. Find actions that lead to goal
  3. Find preconditions of those actions
  4. Recursively solve subgoals
  5. Continue until initial state is reached
```

**Advantages:**
- Often more efficient (fewer states to explore)
- Focuses on relevant states
- Good for goal-driven planning

**Disadvantages:**
- Need to compute inverse of actions
- Multiple goals can be tricky
- Requires knowing what leads to goal

**Example:**
```
Goal: Robot at Room C with Key

Backward Search:
  Robot at Room C
    ← Move from Room B to C
      ← Require: Door1 unlocked
         ← Unlock(Door1)
            ← Require: Have Key
               ← PickUp(Key) in Room A
                  ← Initial state has Key in Room A ✓

Result: Found plan working backward from goal
```

### Comparing Forward and Backward Search

| Aspect | Forward | Backward |
|---|---|---|
| **Starting point** | Initial state | Goal state |
| **Direction** | Toward goal | Toward initial |
| **Efficiency** | Often larger search space | Often smaller |
| **Good for** | Few alternatives initially | Few ways to achieve goal |
| **Implementation** | Straightforward | Needs action inverses |

---

## 2) Partial-Order Planning

### What is Partial-Order Planning?

**Partial-Order Planning (POP)** is a planning approach where actions are NOT forced into one total ordering. Instead, only necessary orderings are specified.

**Contrast with Linear Planning:**

```
Linear Planning:
  Actions: [A, B, C, D, E]
  Order: MUST execute exactly in this sequence
  Constraints: A < B < C < D < E (strict)
  
Partial-Order Planning:
  Actions: {A, B, C, D, E}
  Order: Only necessary constraints specified
  Constraints: 
    - A < D (D depends on A)
    - C < E (E depends on C)
    - B < E (E depends on B)
    - Others can go in any order
  Possible sequences:
    - [A, C, D, B, E] ✓
    - [A, B, C, D, E] ✓
    - [C, A, B, D, E] ✓
```

### Advantages of Partial-Order Planning

```
1. Flexibility
   - Multiple valid orderings possible
   - Can choose best order later
   
2. Efficiency
   - Fewer constraints = faster search
   - Can parallelize independent actions
   
3. Real-world realism
   - Many tasks don't require strict order
   - Allows concurrent execution
   
4. Backtracking efficiency
   - Failed action doesn't invalidate entire plan
   - Can reschedule other actions
```

### Example: Kitchen Task Planning

```
Goal: Prepare dinner (set table, cook food, serve)

Actions:
  1. Prepare vegetables (15 min)
  2. Cook rice (20 min)
  3. Cook curry (25 min)
  4. Set table (10 min)
  5. Serve food (5 min)

Dependencies (Partial Order):
  - Prepare vegetables < Cook curry
    (must prepare before cooking)
  - Cook rice < Serve food
    (rice must be cooked before serving)
  - Cook curry < Serve food
    (curry must be cooked before serving)
  - Set table < Serve food
    (table must be set before serving)

Independent actions (can run in parallel):
  - Prepare vegetables and Cook rice (no dependency)
  - Set table and Cook rice (no dependency)

Valid Partial Orders:
  [Prepare vegetables, Cook rice, Cook curry, Set table, Serve food]
  [Cook rice, Prepare vegetables, Set table, Cook curry, Serve food]
  [Set table, Prepare vegetables, Cook rice, Cook curry, Serve food]
  ... (many more valid orderings)

Optimal Schedule (parallel execution):
  Time 0-15:   Prepare vegetables + Set table (parallel)
  Time 15-20:  Cook rice
  Time 20-25:  Cook curry
  Time 25-30:  Serve food
  Total time: 30 minutes (instead of 75 if all serial)
```

### POP Algorithm Overview

```
Input: Initial state, Goal, Available actions
Output: Partial-order plan

Algorithm:
  1. Start with empty plan
  2. While goal not achieved:
     a. Select unsolved subgoal
     b. Find action that solves it
     c. Add action to plan
     d. Add necessary ordering constraints
     e. Check for conflicts
     f. Resolve conflicts
  3. Return valid partial-order plan
```

### Comparison: Linear vs Partial-Order Planning

| Aspect | Linear | Partial-Order |
|---|---|---|
| **Action order** | Total (A→B→C→D) | Partial (only necessary) |
| **Parallelization** | Not possible | Possible |
| **Search space** | Smaller | Often larger |
| **Flexibility** | Lower | Higher |
| **Implementation** | Simpler | Complex |
| **Efficiency** | Lower (serial) | Higher (parallel) |

---

## 3) Planning Graphs

### What is a Planning Graph?

A **Planning Graph** is a data structure that represents:
- Possible states at each level
- Possible actions at each level
- Mutual exclusion (conflict) relations between actions/states

**Purpose:**
- Estimate if goal is reachable
- Estimate how many steps needed
- Provide heuristics for planning

### Structure of Planning Graph

A planning graph alternates between:

```
Level 0 (State Level):
  All facts true in initial state
  
Level 1 (Action Level):
  All actions applicable to Level 0 facts
  
Level 2 (State Level):
  All facts that can be true after Level 1 actions
  
Level 3 (Action Level):
  All actions applicable to Level 2 facts
  
... and so on
```

### Example: Simple Planning Graph

**Domain:** Robot world

**Initial State (Level 0):**
- At(robot, RoomA)
- Clear(RoomB)
- Key(table)

**Actions available:**
- Move(robot, from, to) - move robot
- PickUp(object) - pick object
- PutDown(object) - put object

**Building the graph:**

```
LEVEL 0 (Initial State):
  Facts: At(R, A), Clear(B), Key(table)

LEVEL 1 (Actions):
  Applicable actions:
    - Move(R, A, B)      [robot can move A→B]
    - Move(R, A, C)      [robot can move A→C] (if C exists)
    - PickUp(key)        [robot can pick key]

LEVEL 2 (Resulting State):
  Facts from Level 1:
    - At(R, B)           [from Move(R, A, B)]
    - At(R, A)           [unchanged]
    - Clear(B)           [unchanged]
    - Key(table)         [unchanged]
    - In-hand(key)       [from PickUp(key)]

LEVEL 3 (New Actions):
  New actions possible with Level 2 state:
    - Move(R, B, A)      [can move back]
    - Move(R, B, C)      [new move option]
    - etc.
```

### Mutual Exclusion in Planning Graphs

**Mutual Exclusion (Mutex)** relations indicate conflicts:

#### **1. Action Mutex**
Two actions conflict if they:
- Have conflicting preconditions
- Have conflicting effects
- Interfere with each other

```
Example:
  Action 1: PickUp(key)
  Action 2: Move(robot, A, B)
  
Check conflicts:
  - Both can be true in same state? YES
  - Effect of 1 conflicts with precondition of 2? NO
  - Effect of 2 conflicts with precondition of 1? NO
  
Conclusion: NOT mutex - can execute together
```

#### **2. Fact Mutex**
Two facts conflict if:
- They're contradictory
- Can't both be true simultaneously

```
Example:
  Fact 1: At(robot, RoomA)
  Fact 2: At(robot, RoomB)
  
Robot cannot be in two places! These are MUTEX.
```

### Planning Graph Expansion Algorithm

```
Algorithm PLANNING-GRAPH(Initial State, Goal):
  
  1. Create Level 0: Add all initial facts
  
  2. Loop:
     a. If goal facts all present and not mutex → SUCCESS
        (Can extract plan)
     
     b. If graph has leveled off (no new facts added):
        → FAILURE (goal unreachable)
     
     c. Create Action Level:
        Add all actions whose preconditions
        are satisfied at current State Level
     
     d. Create next State Level:
        Add all effects of actions
        Propagate facts forward
        Add mutex relations
     
  3. Return level-by-level structure
```

### Using Planning Graphs

#### **For Reachability Analysis**
```
Can we reach goal "At(robot, Room5) ∧ In-hand(key)"?

Check:
  1. Does goal appear in any State Level? YES (Level 4)
  2. Are goal facts mutex? NO
  3. Then: Goal is reachable ✓
```

#### **For Heuristic Estimation**
```
How many steps to reach goal?

Count:
  - First level where ALL goal facts appear = minimum steps

Example:
  Goal facts first appear together in Level 3
  → At least 3 levels of actions needed
  → At least 3 action steps required
  → Heuristic: h(state) = 3
```

---

## 4) Planning and Acting in the Real World

### Why Real-World Planning is Difficult

Real environments differ from textbook problems:

```
Textbook Planning:
  ✓ Complete information known
  ✓ Deterministic actions (always work)
  ✓ Static environment (doesn't change unpredictably)
  ✓ Single agent
  ✓ Perfect sensors

Real World:
  ✗ Incomplete information
  ✗ Uncertain outcomes
  ✗ Dynamic environment
  ✗ Multiple agents
  ✗ Noisy/fallible sensors
  ✗ Time constraints
  ✗ Resource constraints
```

### Challenge 1: Incomplete Information

```
Problem:
  Robot plans route based on map
  Unknown: construction blocked one road
  
Result: Plan fails halfway through

Solution:
  - Online planning (replan as you go)
  - Contingency plans (alternatives)
  - Monitoring and replanning
```

### Challenge 2: Action Uncertainty

```
Problem:
  Action: Move(robot, forward, 10 meters)
  Expected: Robot moves exactly 10 meters
  Reality: Wheel slips, moves only 8 meters
  
Result: Robot is at wrong location

Solution:
  - Probabilistic planning
  - Redundant sensors
  - Error correction
  - Robust control
```

### Challenge 3: Sensing Uncertainty

```
Problem:
  Sensor: Motion detector detects movement
  Reality: Could be mouse or human
  
Result: Wrong action taken

Solution:
  - Sensor fusion (multiple sensors)
  - Error handling
  - Probabilistic reasoning
  - Ask for clarification
```

### Challenge 4: Exogenous Events

```
Problem:
  Robot plans to reach office
  Exogenous event: Elevator breaks
  
Result: Plan cannot execute

Solution:
  - Monitor environment
  - Detect unexpected events
  - Replan immediately
  - Parallel planning threads
```

### Real-World Planning Approach

```
Traditional Approach:
  Plan once → Execute entire plan → Done
  
Real-World Approach:
  Loop:
    1. Perceive environment
    2. Revise world model
    3. Check if plan still valid
    4. If invalid:
       - Replan from current state
       - Or select contingency plan
    5. Execute next action
    6. Observe result
    7. If unexpected:
       - Update knowledge
       - Replan
```

### Example: Autonomous Delivery Robot

```
Initial Plan:
  1. Start at Warehouse
  2. Move to Street1
  3. Move to Street2
  4. Arrive at Customer House
  5. Deliver package

Execution:
  Step 1: Done ✓
  Step 2: Done ✓
  Step 3: Sensor detects: Street2 blocked by construction ✗
  
Real-world Response:
  1. Stop execution
  2. Update world model: "Street2 blocked"
  3. Replan:
     - Alternative: Use Street3 instead
     - Generate new plan
  4. Execute new plan:
     - Move to Street1
     - Move to Street3
     - Move to Alley1
     - Arrive at Customer House
     - Deliver package
```

### Adaptive Planning Strategy

```
Online Planning Process:

While not at goal:
  
  1. Check Preconditions:
     - Are requirements for next action met?
     - YES → Execute action
     - NO → Replan
  
  2. Execute Action:
     - Perform action in environment
     - Observe results
  
  3. Verify Outcomes:
     - Did action have expected effect?
     - YES → Continue
     - NO → Troubleshoot
  
  4. Update Beliefs:
     - Update world model
     - Remember what worked/failed
  
  5. Detect Changes:
     - Unexpected events?
     - Goals changed?
     - Available actions changed?
     → YES: Replan
     → NO: Continue
```

### Example: Medical Treatment Planning

```
Goal: Treat patient's infection

Initial Plan:
  1. Identify organism
  2. Determine antibiotic
  3. Prescribe antibiotics
  4. Monitor patient

Execution with adjustments:
  
  Step 1: Run tests → Identify bacteria ✓
  
  Step 2: Determine antibiotic → Select Penicillin ✓
  
  Step 3: Patient allergic to Penicillin! ✗
         Replan:
         - Select alternative: Ciprofloxacin
         - Prescribe new antibiotic
  
  Step 4: Patient improves? YES ✓
         Continue monitoring
  
  Step 5: New symptom appears? YES ✗
         Adjust treatment
         Possibly restart planning
```

---

## 5) Forward and Backward Chaining

### What is Chaining?

**Chaining** refers to methods of inference where rules are applied in sequence to derive conclusions.

### Forward Chaining

#### **Concept**
Start with known FACTS. Apply RULES to derive new facts. Continue until reaching GOAL.

**Direction:** Data → Conclusion (data-driven)

```
Process:
  1. Start with initial facts
  2. Apply rules that match current facts
  3. Derive new facts
  4. Add new facts to knowledge base
  5. Repeat until goal derived or no new facts
```

#### **Algorithm**

```
ForwardChain(KB, Goal):
  
  Facts = Initial facts in KB
  
  While Goal not in Facts:
    
    For each Rule in KB:
      If Rule.Preconditions ⊆ Facts:
        NewFacts = Rule.Conclusions
        Facts = Facts ∪ NewFacts
    
    If no new facts added:
      Return FAILURE (goal unreachable)
  
  Return SUCCESS (goal derived)
```

#### **Example: Medical Diagnosis (Forward)**

```
Knowledge Base:

Initial Facts:
  - Patient has fever
  - Patient has cough
  - Patient has fatigue
  
Rules:
  R1: If fever ∧ cough → SuspectRespiratory
  R2: If respiratory ∧ fatigue → MayHaveFlu
  R3: If flu → Prescribe(antivirals)
  R4: If antivirals → StayHome

Query: Should patient stay home?

Forward Chaining Execution:

Iteration 1:
  Facts = {fever, cough, fatigue}
  Apply R1: fever ∧ cough present
  → Derive: SuspectRespiratory
  Facts = {fever, cough, fatigue, SuspectRespiratory}

Iteration 2:
  Apply R2: SuspectRespiratory ∧ fatigue present
  → Derive: MayHaveFlu
  Facts = {fever, cough, fatigue, SuspectRespiratory, MayHaveFlu}

Iteration 3:
  Apply R3: MayHaveFlu present
  → Derive: Prescribe(antivirals)
  Facts = {..., Prescribe(antivirals)}

Iteration 4:
  Apply R4: Prescribe(antivirals) present
  → Derive: StayHome
  Facts = {..., StayHome}

Goal StayHome reached!
Answer: YES, patient should stay home
```

### Backward Chaining

#### **Concept**
Start with GOAL. Find RULES that produce goal. Work backward to FACTS.

**Direction:** Conclusion ← Data (goal-driven)

```
Process:
  1. Start with goal
  2. Find rules that conclude goal
  3. Check if rule preconditions are in KB
  4. If not, recursively solve preconditions as subgoals
  5. Continue until reaching base facts
```

#### **Algorithm**

```
BackwardChain(KB, Goal):
  
  If Goal in Facts(KB):
    Return TRUE
  
  For each Rule where Rule.Conclusion = Goal:
    
    Preconditions = Rule.Preconditions
    
    If BackwardChain(KB, Precondition1) AND
       BackwardChain(KB, Precondition2) AND ...
      
      Return TRUE
  
  Return FALSE
```

#### **Example: Same Medical Diagnosis (Backward)**

```
Query: Is StayHome true?

Backward Chaining Execution:

Goal: StayHome?
  Find rule concluding StayHome:
    R4: If Prescribe(antivirals) → StayHome
  
  New Subgoal: Prescribe(antivirals)?
    Find rule concluding Prescribe(antivirals):
      R3: If MayHaveFlu → Prescribe(antivirals)
    
    New Subgoal: MayHaveFlu?
      Find rule concluding MayHaveFlu:
        R2: If SuspectRespiratory ∧ fatigue → MayHaveFlu
      
      New Subgoal 1: SuspectRespiratory?
        Find rule:
          R1: If fever ∧ cough → SuspectRespiratory
        
        New Subgoal 1a: fever? → YES (in facts) ✓
        New Subgoal 1b: cough? → YES (in facts) ✓
        → SuspectRespiratory true
      
      New Subgoal 2: fatigue? → YES (in facts) ✓
      
      → MayHaveFlu true
    
    → Prescribe(antivirals) true
  
  → StayHome true

Answer: YES
```

### Comparison: Forward vs Backward Chaining

| Aspect | Forward | Backward |
|---|---|---|
| **Start** | Initial facts | Goal |
| **Direction** | Data → Conclusion | Conclusion ← Data |
| **Search** | All applicable rules | Only relevant rules |
| **Problem** | Many irrelevant derivations | Focused on goal |
| **Space** | Larger | Smaller |
| **Time** | May be slower | Efficient for specific goals |
| **Use Case** | Monitoring, diagnosis from symptoms | Planning, proof verification |
| **Derivation** | Bottom-up | Top-down |

### Backward Chaining Proof Tree

```
              StayHome
                 |
            Prescribe(antivirals)
                 |
             MayHaveFlu
              /       \
      SuspectRespiratory  Fatigue
         /       \            |
       Fever    Cough        YES ✓
        |         |
       YES ✓    YES ✓

All leaves satisfied → Goal proven!
```

---

## 6) Unification

### What is Unification?

**Unification** is the process of making two logical expressions identical by finding appropriate variable substitutions.

**Simple Concept:**
```
Unification = Making two expressions match by substituting variables
```

### Unification Process

#### **Basic Example**

```
Expression 1: Loves(x, mother(x))
Expression 2: Loves(john, mother(john))

Unification:
  Match: Loves(x, mother(x)) with Loves(john, mother(john))
  
  Variable x must match constant john
  Substitution: x = john
  
  Check: Loves(john, mother(john)) = Loves(john, mother(john)) ✓
  
Result: Unification SUCCEEDS with substitution {x/john}
```

#### **Failure Case**

```
Expression 1: Knows(x, x)       [Someone knows themselves]
Expression 2: Knows(john, mary)

Unification:
  Match: Knows(x, x) with Knows(john, mary)
  
  First x must be = john
  Second x must be = mary
  But x cannot be both john AND mary!
  
Result: Unification FAILS
```

### Most General Unifier (MGU)

A **Most General Unifier** is the most general substitution that makes two expressions identical.

```
Example 1:
  Expression 1: Parent(x, y)
  Expression 2: Parent(john, mary)
  
  MGU: {x/john, y/mary}
  Result: Parent(john, mary)

Example 2:
  Expression 1: Parent(x, y)
  Expression 2: Parent(john, y)
  
  MGU: {x/john}
  Result: Parent(john, y)
  (y remains variable, not fixed to specific value)

Example 3:
  Expression 1: Parent(x, child(x))
  Expression 2: Parent(john, child(john))
  
  MGU: {x/john}
  Result: Parent(john, child(john))
```

### Unification Algorithm

```
Unify(Term1, Term2, Substitution):
  
  1. Apply current substitution to both terms
  
  2. If Term1 = Term2 (identical):
     Return Substitution (success)
  
  3. If Term1 is variable:
     If occurs_check(Term1, Term2):
       Return FAILURE (infinite structure)
     Else:
       Return Unify(Term2, Term1, Substitution ∪ {Term1/Term2})
  
  4. If Term2 is variable:
     If occurs_check(Term2, Term1):
       Return FAILURE
     Else:
       Return Unify(Term1, Term2, Substitution ∪ {Term2/Term1})
  
  5. If both compound (f(a,b) form):
     If function symbols differ:
       Return FAILURE
     If arity differs:
       Return FAILURE
     Else:
       For each argument pair:
         Recursively unify
       Return combined substitutions
  
  6. If one atom, one variable:
     Apply variable substitution rule
  
  Return FAILURE
```

### Practical Examples

#### **Example 1: Pattern Matching**

```
Pattern: Student(name, age, university)
Fact: Student(alice, 20, MIT)

Unification:
  name = alice
  age = 20
  university = MIT
  
Result: Pattern matches fact ✓
```

#### **Example 2: Function Composition**

```
Expression 1: Apply(f, Apply(g, x))
Expression 2: Apply(f, Apply(h, 5))

Unification:
  f = f ✓
  Apply(g, x) = Apply(h, 5)
    g ≠ h ✗
    
Result: FAILURE (cannot unify)
```

#### **Example 3: Complex Structure**

```
Expression 1: Ancestor(X, Father(john))
Expression 2: Ancestor(Mary, Y)

Unification:
  X = Mary
  Father(john) = Y
  
MGU: {X/Mary, Y/Father(john)}

After unification:
  Ancestor(Mary, Father(john))
```

### Occurs Check

The **occurs check** prevents infinite structures:

```
Problem without occurs check:
  Expression 1: f(x)
  Expression 2: x
  
Unification attempt:
  x = f(x) ???
  But if x = f(x), then:
    x = f(f(x)) = f(f(f(x))) = ... (infinite!)

Solution: Occurs check
  If variable occurs in term:
    Return FAILURE
    
With occurs check:
  x occurs in f(x)
  → Return FAILURE ✓
```

### Why Unification Matters

```
1. FOL Inference
   - Essential for universal instantiation
   - Allows applying general rules to specific cases
   
2. Prolog Programming
   - Core mechanism of pattern matching
   - Enables backtracking search
   
3. Automated Reasoning
   - Makes resolution work in FOL
   - Enables theorem proving
   
4. Logic Programming
   - Foundation of declarative programming
   - Enables queries and logical reasoning
```

---

## 7) Resolution

### What is Resolution?

**Resolution** is a single inference rule that is both:
- **Sound** (only derives true conclusions)
- **Complete** (can derive any logical consequence)

It forms the basis of automated theorem proving.

### Resolution in Propositional Logic

#### **Basic Idea**

Combine two clauses with complementary literals to produce a new clause.

**Form:**
```
Clause 1: A ∨ B      [A or B]
Clause 2: ¬B ∨ C     [not B or C]
________________
Resolvent: A ∨ C     [A or C]

Explanation:
  - If B is true: C must be true (from Clause 2)
  - If B is false: A must be true (from Clause 1)
  - Therefore: A or C must be true
```

#### **Examples**

**Example 1:**
```
Clause 1: Rain ∨ Sprinkler
Clause 2: ¬Rain ∨ GroundWet

Resolution:
  Eliminate Rain and ¬Rain
  Result: Sprinkler ∨ GroundWet
  
Meaning: Either sprinkler is on or ground got wet
```

**Example 2:**
```
Clause 1: Happy ∨ Rich
Clause 2: ¬Happy ∨ Famous

Resolution:
  Result: Rich ∨ Famous
  
Meaning: Person is either rich or famous (or both)
```

**Example 3 (Contradiction):**
```
Clause 1: Guilty
Clause 2: ¬Guilty

Resolution:
  Result: □ (empty clause, FALSE)
  
Meaning: Contradiction! Not both Guilty and ¬Guilty
```

### Resolution in First-Order Logic

Resolution extends to FOL using **unification**.

#### **FOL Resolution**

```
Clause 1: A ∨ ... ∨ L1
Clause 2: B ∨ ... ∨ L2

If L1 and L2 can be unified:
  L1 = P(x, y)
  L2 = ¬P(a, b)
  
  MGU: {x/a, y/b}
  
  Then resolve, substitute, and simplify
```

#### **Example: FOL Resolution**

```
Clause 1: Mortal(x) ∨ Divine(x)
Clause 2: ¬Mortal(socrates)

FOL Resolution:
  Unify Mortal(x) in Clause 1 with Mortal(socrates) in Clause 2
  MGU: {x/socrates}
  
  Apply to Clause 1: Mortal(socrates) ∨ Divine(socrates)
  
  Resolve with Clause 2:
    Mortal(socrates) cancels with ¬Mortal(socrates)
  
  Result: Divine(socrates)
  
Conclusion: Socrates is divine
```

### Proof by Contradiction (Refutation)

Resolution is often used for **proof by contradiction**:

```
To prove: Goal G

Steps:
  1. Assume ¬G (negate goal)
  2. Add ¬G to knowledge base
  3. Apply resolution repeatedly
  4. If we derive empty clause □ → Contradiction!
  5. Therefore G must be true
```

#### **Example: Proof by Contradiction**

```
Knowledge Base:
  1. Socrates is human: Human(socrates)
  2. All humans are mortal: ∀x (Human(x) → Mortal(x))

Goal: Prove Mortal(socrates)

Proof:
  1. Assume ¬Mortal(socrates)  [negation of goal]
  
  2. Convert KB to clauses:
     Clause A: Human(socrates)
     Clause B: ¬Human(x) ∨ Mortal(x)
                [equivalent to Human(x) → Mortal(x)]
     Clause C: ¬Mortal(socrates)  [negation of goal]
  
  3. Resolution:
     Step 1: Resolve B and A
       ¬Human(x) ∨ Mortal(x) with Human(socrates)
       Unify x = socrates
       Result: Mortal(socrates)
     
     Step 2: Resolve with C
       Mortal(socrates) with ¬Mortal(socrates)
       Result: □ (empty clause - CONTRADICTION!)
  
  4. Conclusion: 
     ¬Mortal(socrates) leads to contradiction
     Therefore: Mortal(socrates) is TRUE ✓
```

### Resolution Algorithm

```
Resolution-Refutation(KB, Goal):
  
  1. Negate goal: NotGoal = ¬Goal
  
  2. Convert to CNF (Conjunctive Normal Form):
     Convert KB and NotGoal to clause form
  
  3. Clauses = all clauses from KB + NotGoal
  
  4. While not done:
     
     a. Select two clauses from Clauses
     
     b. Try to resolve them:
        If resolvent can be produced:
          If resolvent = □ (empty):
            RETURN SUCCESS (goal proven)
          Else:
            Add resolvent to Clauses
     
     c. If no new clauses added:
        RETURN FAILURE (goal not provable)
  
  5. RETURN FAILURE
```

### Why Resolution is Important

```
1. Completeness
   - Can prove any logical consequence
   - If goal is true, resolution will find it
   
2. Simplicity
   - Only one inference rule
   - Mechanically applicable
   - Easy to implement
   
3. Automation
   - Basis of automated theorem provers
   - Can be programmed for computers
   
4. Theoretical Foundation
   - Proves that logic is decidable
   - Shows limitations of logic
```

---

## 8) Quantifying Uncertainty

### Why Uncertainty?

Perfect information is rare in real world:

```
Examples of Uncertainty:

1. Incomplete Information:
   - Medical diagnosis: Not all test results available
   - Sensor data: Sensor may not detect everything
   
2. Measurement Errors:
   - GPS: Location off by few meters
   - Thermometer: Temperature measurement not exact
   
3. Inherent Randomness:
   - Weather: Cannot predict exactly
   - Coin flip: Inherently random outcome
   
4. Complexity:
   - Traffic prediction: Too many variables to track
   - Stock market: Complex interdependencies
```

### Representing Uncertainty

#### **1. Probability**

Most common: Assign probability to propositions

```
P(Rain) = 0.7  [70% chance of rain]
P(Flu | Fever) = 0.8  [80% chance of flu given fever]
```

#### **2. Certainty Factors**

Confidence measure (older AI systems):
```
CF = confidence factor from -1 (definitely false) to 1 (definitely true)
```

#### **3. Fuzzy Logic**

Degrees of membership:
```
"Temperature is hot" = 0.7  [70% membership in "hot" category]
```

#### **4. Dempster-Shafer Theory**

Belief and plausibility:
```
Belief(A) = how much we believe A
Plausibility(A) = how much we don't disbelieve A
```

### Probability Basics

#### **Prior Probability**
```
P(Disease) = probability before any evidence
Example: P(Flu) = 0.01 (1% of population has flu)
```

#### **Conditional Probability**
```
P(A|B) = probability of A given B
Example: P(Flu | Fever) = 0.8
  "If patient has fever, 80% chance they have flu"
```

#### **Joint Probability**
```
P(A, B) = probability of both A and B
Example: P(Fever, Flu) = probability patient has both fever and flu
```

### Why Quantifying Uncertainty Matters

```
Medical Diagnosis:
  Without: "Patient has flu" → Too confident, may be wrong
  With probability: "Patient has flu with 75% probability"
            → More nuanced, better action possible
            
Autonomous Driving:
  Without: "Road is clear" → May crash if miss obstacle
  With probability: "Road is 95% clear, 5% obstacle"
            → Can plan accordingly, be cautious
            
Spam Detection:
  Without: "Email is spam" → Lose legitimate emails
  With probability: "Email is 90% likely spam"
            → Can flag for review or use threshold
```

---

## 9) Probabilistic Reasoning

### What is Probabilistic Reasoning?

Probabilistic reasoning uses probabilities to:
- Represent uncertainty
- Combine evidence
- Make decisions
- Update beliefs

### Key Concepts

#### **Bayes' Theorem**

The most important formula in probabilistic reasoning:

```
P(H|E) = P(E|H) × P(H) / P(E)

Where:
  H = Hypothesis
  E = Evidence
  P(H|E) = Posterior probability (what we want to know)
  P(E|H) = Likelihood (how likely evidence if hypothesis true)
  P(H) = Prior probability (before evidence)
  P(E) = Total probability of evidence
```

**Intuition:**
```
Before evidence: P(H) - our prior belief
After evidence E:
  - If E strongly supports H: P(H|E) increases
  - If E contradicts H: P(H|E) decreases
  - If E irrelevant: P(H|E) ≈ P(H)
```

#### **Example: Medical Diagnosis with Bayes**

```
Disease: Flu
Evidence: Patient has fever

Prior Knowledge:
  P(Flu) = 0.01  [1% population has flu]
  P(Fever) = 0.05  [5% have fever (for any reason)]
  P(Fever|Flu) = 0.9  [90% of flu patients have fever]

Query: If patient has fever, what's probability they have flu?

Calculation:
  P(Flu|Fever) = P(Fever|Flu) × P(Flu) / P(Fever)
               = 0.9 × 0.01 / 0.05
               = 0.009 / 0.05
               = 0.18 or 18%

Interpretation:
  Even though 90% of flu patients have fever,
  Only 18% of fever patients actually have flu!
  Because flu is rare (1%) compared to fever causes (5%)
```

#### **Another Example: Spam Detection**

```
Hypothesis: Email is spam
Evidence: Email contains word "FREE"

Data:
  P(Spam) = 0.3  [30% of emails are spam]
  P("FREE" in email) = 0.1  [10% of all emails have "FREE"]
  P("FREE"|Spam) = 0.8  [80% of spam have "FREE"]

Query: If email contains "FREE", is it spam?

Calculation:
  P(Spam|"FREE") = P("FREE"|Spam) × P(Spam) / P("FREE")
                 = 0.8 × 0.3 / 0.1
                 = 0.24 / 0.1
                 = 2.4 → IMPOSSIBLE! (probability > 1)
  
Wait, need to recalculate P("FREE"):
  P("FREE") = P("FREE"|Spam)×P(Spam) + P("FREE"|¬Spam)×P(¬Spam)
            = 0.8 × 0.3 + 0.05 × 0.7
            = 0.24 + 0.035
            = 0.275

Correct calculation:
  P(Spam|"FREE") = 0.8 × 0.3 / 0.275
                 = 0.24 / 0.275
                 ≈ 0.87 or 87%

Interpretation:
  If email contains "FREE", 87% likely it's spam
```

### Belief Networks (Bayesian Networks)

A **Belief Network** is a directed graph showing:
- Random variables (nodes)
- Conditional dependencies (edges)
- Probability tables

**Example: Rain affects sprinkler and ground wetness**

```
        Rain
        /  \
       /    \
   Sprinkler GroundWet

Meaning:
  - Rain affects whether sprinkler is on
  - Rain and Sprinkler both affect GroundWet
  - (assuming smart controller: doesn't water in rain)

Probability Tables:
  P(Rain) = 0.2
  P(Sprinkler|Rain) = 0.1  [if raining, less likely to water]
  P(Sprinkler|¬Rain) = 0.8  [if dry, likely to water]
  P(GroundWet|Rain,Sprinkler) = 0.99
  P(GroundWet|Rain,¬Sprinkler) = 0.8
  P(GroundWet|¬Rain,Sprinkler) = 0.9
  P(GroundWet|¬Rain,¬Sprinkler) = 0.0
```

### Inference in Belief Networks

#### **Exact Inference**

Compute exact probabilities using network structure

#### **Approximate Inference**

Use sampling or heuristics for large networks

### Conditional Independence

**Key insight for efficient reasoning:**

```
Conditional Independence:
  A is independent of C given B if:
  P(A|B,C) = P(A|B)
  
Example:
  Toothache independent of Weather given DrillBit?
  - If I know drill bit hit nerve → toothache
  - Weather doesn't matter
  - So: P(Toothache|DrillBit, Weather) = P(Toothache|DrillBit)
```

---

## 10) Probabilistic Reasoning Over Time

### Why Time Matters

Many real-world systems evolve over time:

```
Examples:
  - Robot position changes as it moves
  - Patient health status changes with treatment
  - Stock price changes daily
  - Weather changes continuously
  - Traffic conditions evolve
```

### Temporal Models

#### **1. Markov Assumption**

**Markov Property:**
```
The future depends ONLY on the present, not the entire past

P(S_{t+1} | S_t, S_{t-1}, ..., S_0) = P(S_{t+1} | S_t)

Meaning:
  State at time t+1 depends only on state at time t
  History before t is irrelevant
```

**Example:**
```
Where is robot tomorrow?
  With Markov: Depends only on position today
  Without Markov: Would need entire history
  
Why Markov helps:
  - Massive reduction in state space
  - Efficient inference possible
  - Often reasonable approximation
```

#### **2. Markov Chain**

Simple model of state transitions:

```
States: S_0 → S_1 → S_2 → S_3 → ...

Transition probabilities:
  P(S_1 | S_0)
  P(S_2 | S_1)
  P(S_3 | S_2)
  ...

Example - Weather Model:
  Today's weather: Sunny, Rainy, Cloudy
  
  Transition probabilities:
    P(Sunny tomorrow | Sunny today) = 0.8
    P(Rainy tomorrow | Sunny today) = 0.1
    P(Cloudy tomorrow | Sunny today) = 0.1
    
    P(Sunny tomorrow | Rainy today) = 0.3
    P(Rainy tomorrow | Rainy today) = 0.4
    P(Cloudy tomorrow | Rainy today) = 0.3
    
    ... (and so on)
```

#### **3. Hidden Markov Model (HMM)**

Model where:
- **Hidden states** are not directly observable
- **Observations** give clues about hidden states

**Structure:**

```
Hidden states:  S_0 → S_1 → S_2 → S_3 → ...
                 ↓     ↓     ↓     ↓
Observations:   O_0   O_1   O_2   O_3
```

**Example: Speech Recognition**

```
Hidden states:  Phonemes (actual sounds)
Observations:   Audio signal (what microphone records)

Process:
  Speaker says "hello"
  Hidden state sequence: h-e-l-l-o
  Observation sequence: noisy audio signal
  
Task: Infer correct phonemes from noisy audio
```

**Example: Traffic Monitoring**

```
Hidden states:  Actual traffic congestion (unknown directly)
Observations:   Number of cars detected by sensor

Scenario:
  Sensor reports: 50 cars passing
  But hidden state: Heavy congestion
  
Why hidden?
  - Cannot directly see all traffic
  - Only indirect measurements via sensor
  - Multiple states could produce same observation
```

#### **4. Filtering (State Estimation)**

Given observations up to time t, estimate state at time t

```
Algorithm:
  For each time step:
    1. Predict: Compute P(S_t | observations 0..t-1)
    2. Observe: Get observation O_t
    3. Update: Compute P(S_t | observations 0..t)
    4. Shift time
    5. Repeat
```

**Example: Robot Localization**

```
Time 0:
  Robot at unknown location
  Observation: Sensor reading 100m to wall
  
Time 1:
  Robot moves forward 5m
  Prediction: Probably 95m to wall
  Observation: Sensor reads 95m
  Update: Very confident position estimate ✓
  
Time 2:
  Robot moves left 3m
  Prediction: Probably 95m straight, 3m left
  Observation: Sensor reads 96m (slight noise)
  Update: Refine position estimate
```

#### **5. Prediction**

Estimate future state given current observations

```
Query: Given observations 0..t, what will state be at t+k?

Answer: P(S_{t+k} | observations 0..t)

Process:
  1. Use current observations to estimate current state
  2. Project forward k steps using transition model
  3. Result: Probability distribution over future states
```

**Example: Stock Price Prediction**

```
Historical data: Stock prices over past year
Observation: Today's price $100

Markov chain model:
  P(Price tomorrow | Price today)
  (learned from historical data)

Prediction:
  Q: What's price next week?
  A: ~$105 with 60% confidence (based on model)
```

#### **6. Smoothing (Retroactive Inference)**

Estimate past state given observations including future observations

```
Query: What was the true state at time t, 
       given observations 0..T (where T > t)?

Difference from filtering:
  Filtering: Estimate current state using past observations only
  Smoothing: Estimate any past state using all observations
```

**Example: Medical Diagnosis**

```
Filtering (real-time):
  Given fever observed today → What disease likely now?
  
Smoothing (after getting more data):
  Observed: Day 1: Fever, Day 2: Fever + Cough, Day 3: Recovery
  Question: What disease on Day 1?
  Answer: More confident now (can see full pattern)
```

### Kalman Filter

Special efficient algorithm for HMM with:
- Continuous states
- Linear transitions
- Gaussian distributions

**Used in:**
- Radar tracking
- Navigation systems
- Sensor fusion

---

# Summary of Unit III

| Topic | Key Concept |
|---|---|
| **Planning with State-Space** | Finding action sequences from initial to goal state |
| **Partial-Order Planning** | Only necessary action orderings specified |
| **Planning Graphs** | Layered representation for reachability analysis |
| **Real-World Planning** | Adaptive replanning for uncertain, dynamic environments |
| **Forward Chaining** | Data-driven inference (facts → conclusions) |
| **Backward Chaining** | Goal-driven inference (goals ← facts) |
| **Unification** | Matching expressions by variable substitution |
| **Resolution** | Inference rule for automated reasoning |
| **Uncertainty** | Representing incomplete/uncertain knowledge |
| **Probabilistic Reasoning** | Using probabilities with Bayes' theorem |
| **Temporal Models** | Reasoning about systems that change over time |
