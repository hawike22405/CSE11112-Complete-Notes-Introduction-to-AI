Unit_I_AI_Agents_Search.md

md
# Unit I: Artificial Intelligence, Agents, Search, Heuristics, Constraints

## 1) Introduction to Artificial Intelligence

### Definition
**Artificial Intelligence (AI)** is the branch of computer science that designs and develops intelligent machines and systems capable of performing tasks that typically require human intelligence.

### What is Intelligence?
An intelligent system demonstrates:
- **Logical thinking** → Making valid conclusions from facts
- **Learning** → Improving from past experiences
- **Problem-solving** → Finding solutions to complex issues
- **Decision-making** → Choosing best actions
- **Language understanding** → Comprehending and generating human language
- **Adaptation** → Adjusting to new situations and environments

### Real-World AI Applications

| Application | How AI Works |
|-------------|-------------|
| **Chess Playing** | Evaluates millions of moves, learns from patterns |
| **Face Recognition** | Uses neural networks to identify facial features |
| **Movie Recommendations** | Analyzes user behavior and preferences |
| **Voice Assistants** | Processes speech and understands intent |
| **Autonomous Vehicles** | Perceives environment and makes driving decisions |
| **Medical Diagnosis** | Analyzes symptoms and medical images |
| **Email Spam Filter** | Classifies emails as spam or legitimate |
| **Language Translation** | Converts text between languages using deep learning |

### Main Goals of AI

1. **Build Expert Systems**
   - Systems that solve domain-specific problems like medical diagnosis or financial planning
   - Example: Medical expert systems that diagnose diseases based on symptoms

2. **Create Intelligent Agents**
   - Autonomous systems that sense environment and act accordingly
   - Example: Robots that clean homes or explore planets

3. **Enable Learning from Data**
   - Systems that improve automatically without explicit programming
   - Example: Netflix learning what movies you like

4. **Support Reasoning and Decision-Making**
   - Systems that make intelligent choices under uncertainty
   - Example: Traffic management systems

### Types of AI

#### 1. Narrow AI (Weak AI)
- Designed to perform **one specific task**
- Cannot transfer knowledge to other domains
- **Examples:**
  - Image recognition system
  - Chess-playing program (Deep Blue)
  - Language translation tool
  - Voice assistant (Alexa, Siri)

#### 2. General AI (Strong AI)
- Would have human-like intelligence across **many different tasks**
- Could understand any intellectual task a human can
- Currently **theoretical** — does not exist yet
- Would be able to learn, adapt, and apply knowledge across domains

#### 3. Super AI (ASI)
- Would exceed human intelligence in all aspects
- Still purely hypothetical
- Subject of science fiction

### AI Implementation Approaches

┌─────────────────────────────────────┐ │ WAYS TO BUILD AI SYSTEMS │ ├─────────────────────────────────────┤ │ 1. Symbolic Reasoning │ │ → Uses logic and rules │ │ → Example: Expert systems │ │ │ │ 2. Search Algorithms │ │ → Explores possible solutions │ │ → Example: Chess algorithms │ │ │ │ 3. Knowledge Representation │ │ → Stores facts in structured way│ │ → Example: Knowledge graphs │ │ │ │ 4. Machine Learning │ │ → Learns from data │ │ → Example: Neural networks │ │ │ │ 5. Probabilistic Reasoning │ │ → Uses probability theory │ │ → Example: Bayesian networks │ │ │ │ 6. Planning & Optimization │ │ → Plans sequences of actions │ │ → Example: Robot path planning │ └─────────────────────────────────────┘

Code

---

## 2) Agents

### Definition
An **agent** is an autonomous system that:
- **Perceives** its environment through sensors
- **Thinks/Reasons** using its knowledge
- **Acts** on the environment through actuators to achieve goals

### Agent Architecture Diagram

┌──────────────────────────────────────┐ │ AGENT ARCHITECTURE │ ├──────────────────────────────────────┤ │ │ │ PERCEPTION DECISION │ │ ┌─────────┐ ┌──────────┐ │ │ │ Sensors │──→ │ Decision │ ──→ │ Actuators │ │ └─────────┘ │ Module │ │ │ │ └──────────┘ │ Wheels, │ │ ▲ │ Motors, │ │ │ │ etc. │ │ ┌──────────────┐ │ │ │ │ Knowledge & │ └─────────┘ │ │ Memory Base │ │ │ └──────────────┘ │ │ ▲ │ │ │ │ │ └───────────────────┘ │ (Feedback Loop) └──────────────────────────────────────┘

Code

### Agent Function

The agent follows this principle:
Percept Sequence → Action (What it senses) → (What it does)

Code

**Example:** Robot Vacuum
- **Percept:** Detects dirt, wall, obstacle
- **Action:** Start cleaning, turn, stop

### Rational Agent

**Definition:** A rational agent is one that performs actions to maximize its expected performance based on:
- What it has perceived
- Its knowledge about the world
- Expected outcomes of its actions

**Key Formula:**
Rational Action = Best action that maximizes expected utility

Code

**Example: Job Selection**
A rational student choosing between 2 job offers:
- Job A: Salary ₹500k, location far, growth high
- Job B: Salary ₹600k, location near, growth low

Rational choice = Job that gives maximum overall satisfaction

### Five Types of Agents

#### 1. Simple Reflex Agent
- **Works on:** Current perception only
- **Memory:** No past information
- **Best for:** Simple, predictable environments

**Example:**
IF obstacle ahead THEN turn

IF ball is red THEN chase ball

Code

#### 2. Model-Based Reflex Agent
- **Maintains:** Internal model of world state
- **Memory:** Keeps track of world changes
- **Better for:** Partially observable environments

**Example: A car agent**
State = position, speed, surrounding vehicles IF speed > limit AND police ahead THEN brake

Code

#### 3. Goal-Based Agent
- **Has explicit goals**
- **Plans:** Sequences of actions to achieve goals
- **Uses:** Search and planning

**Example: GPS Navigation**
- **Goal:** Reach destination
- **Actions:** Turn right, go straight, turn left
- **Planning:** Creates route from current location to destination

#### 4. Utility-Based Agent
- **Has preferences** among goals
- **Maximizes:** Utility (satisfaction/benefit)
- **Handles trade-offs:** Between competing objectives

**Example: Autonomous Vehicle**
Objectives:

Reach destination (utility = +100)
Avoid accidents (utility = +200)
Minimize time (utility = +50)
Minimize fuel (utility = +30)
Action chosen = one maximizing total utility

Code

#### 5. Learning Agent
- **Improves** performance from experience
- **Components:**
  - Learning element (updates from feedback)
  - Performance element (executes actions)
  - Critic (evaluates performance)
  - Problem generator (finds new opportunities)

**Example: Email Spam Filter**
- Learns from marked spam/legitimate emails
- Continuously improves accuracy
- Adapts to new spam patterns

### Comparison Table

| Type | Memory | Goal | Learns | Best Used For |
|------|--------|------|--------|---------------|
| Simple Reflex | No | No | No | Simple rules |
| Model-Based | Yes | No | No | Stateful systems |
| Goal-Based | Yes | Yes | No | Planning tasks |
| Utility-Based | Yes | Yes (multiple) | No | Complex trade-offs |
| Learning | Yes | Yes | **Yes** | Adaptive systems |

---

## 3) Problem Formulation

### Why Problem Formulation Matters?
Before an AI system can solve a problem, it must be **clearly defined**. A poorly formulated problem leads to:
- Wrong solutions
- Wasted computational resources
- Inefficient search

### Components of Problem Definition

Every AI problem has 5 essential components:

#### 1. Initial State
- **Definition:** Starting point or condition
- **Example:** 
  - Navigation: "User is at Home"
  - Puzzle: "8-puzzle in scrambled state"
  - Game: "Chess board with starting position"

#### 2. Goal State
- **Definition:** Target or desired outcome
- **Example:**
  - Navigation: "Reach Office"
  - Puzzle: "8-puzzle numbers in order"
  - Game: "Checkmate opponent"

#### 3. Actions/Operators
- **Definition:** Possible moves or transitions
- **Example:**
  - Navigation: Move to adjacent city
  - Puzzle: Slide a tile
  - Game: Move piece legally

#### 4. Transition Model
- **Definition:** What happens after each action
- **Example:**
  - Navigation: "If move to City-B from City-A, then current = City-B"
  - Puzzle: "If slide tile, then positions swap"

#### 5. Path Cost
- **Definition:** Cost associated with each move
- **Example:**
  - Navigation: Distance or time to travel
  - Puzzle: Number of moves
  - Game: Strategic value of move

### Complete Problem Formulation Example

**Problem: Navigating from Delhi to Goa**

Initial State: Location = Delhi, Fuel = Full Goal State: Location = Goa, within 24 hours Actions: Go to adjacent city via road/flight Transition: Move to new city, update fuel/time Path Cost: Distance (km) or Time (hours)

Code

### Another Example: 8-Puzzle Problem

Initial State: 1 2 3 4 _ 5 7 8 6

Goal State: 1 2 3 4 5 6 7 8 _

Actions: Move blank tile (UP, DOWN, LEFT, RIGHT) Path Cost: 1 per move (total moves to solution)

Code

### State Space Representation

Initial State (Start) ↓ Action 1 Action 2 Action 3 ↓ ↓ ↓ State 1.1 State 1.2 State 1.3 ↓ ↓ ↓ Action ...continues... ↓ Goal State (Success!)

Code

---

## 4) Uninformed Search Strategies

Uninformed searches (also called **blind search**) explore the problem space without domain-specific knowledge. They only know:
- Initial state
- Available actions
- Goal test

### a) Breadth-First Search (BFS)

**How it works:**
- Explores all nodes at depth d before exploring depth d+1
- Uses a **QUEUE** (FIFO — First In First Out)
- Level by level expansion

**Visual Example:**
Starting at A, find path to G

Code
    A ← Start
   /|\
  B C D
 /| |
E F G ← Goal
BFS Order: A → B, C, D → E, F, G

Finds G in 2 steps

Code

**Algorithm:**
Create queue, add initial state
While queue not empty: a. Dequeue node b. If it's goal, return SUCCESS c. Generate all children d. Enqueue all children
Return FAILURE
Code

**Advantages:**
✓ Complete (always finds solution if it exists)
✓ Optimal for uniform cost (all edges = 1)
✓ Finds shortest path in terms of number of steps

**Disadvantages:**
✗ High memory usage (exponential)
✗ Slow for large search spaces
✗ Time complexity: O(b^d) where b=branching factor, d=depth

**When to use:**
- Small problem spaces
- When shortest path is important
- When memory is not a constraint

---

### b) Depth-First Search (DFS)

**How it works:**
- Explores one branch completely before backtracking
- Uses a **STACK** (LIFO — Last In First Out)
- Goes as deep as possible first

**Visual Example:**
Starting at A, find path to G

Code
    A ← Start
   /|\
  B C D
 /| |
E F G ← Goal
DFS Order: A → B → E → F → C → D → G

May explore deep before finding goal

Code

**Algorithm:**
Create stack, add initial state
While stack not empty: a. Pop node b. If it's goal, return SUCCESS c. Generate all children d. Push all children to stack
Return FAILURE
Code

**Advantages:**
✓ Low memory usage (only stores current path)
✓ Simple to implement
✓ Good for checking if solution exists

**Disadvantages:**
✗ May go infinitely deep (not complete in infinite graphs)
✗ Not optimal (may find longer path)
✗ Slower than BFS for certain problems

**Time Complexity:** O(b^m) where m = maximum depth

---

### c) Uniform Cost Search (UCS)

**How it works:**
- Expands node with **lowest path cost first**
- Uses a **PRIORITY QUEUE**
- Always explores cheapest path

**Visual Example:**
Network with costs:

Code
A -1- B -2- D
|          
3          
|          
C -2- D
UCS finds: A → B → D (cost=3) Not: A → C → D (cost=5)

Code

**Algorithm:**
Create priority queue with initial state (cost=0)
While queue not empty: a. Pop node with minimum cost b. If it's goal, return SUCCESS c. Generate children with new costs d. Add to priority queue
Return FAILURE
Code

**Advantages:**
✓ Complete
✓ Optimal (finds minimum cost path)
✓ Works with non-uniform costs

**Disadvantages:**
✗ Slower than BFS if costs are similar
✗ High memory usage

---

### d) Depth-Limited Search (DLS)

**How it works:**
- DFS with a **maximum depth limit**
- Prevents infinite exploration

**Example:** Limit depth to 5
Only explore up to 5 levels deep

Code

**Advantages:**
✓ Prevents infinite loops
✓ Lower memory than BFS

**Disadvantages:**
✗ May miss solutions beyond the limit

---

### e) Iterative Deepening Search (IDS)

**How it works:**
- Repeatedly performs DFS with increasing depth limits
- Depth limit 1, then 2, then 3, etc.

**Example:**
Iteration 1: DFS with depth ≤ 1 Iteration 2: DFS with depth ≤ 2 Iteration 3: DFS with depth ≤ 3 ...until goal found

Code

**Advantages:**
✓ Complete like BFS
✓ Memory efficient like DFS
✓ Optimal for uniform costs

**Disadvantages:**
✗ Re-explores same nodes multiple times
✗ Slower due to redundant exploration

**Complexity:** O(b^d) time, O(d) space

### Comparison Table

| Algorithm | Complete | Optimal | Time | Space | Best For |
|-----------|----------|---------|------|-------|----------|
| BFS | Yes | Yes* | O(b^d) | O(b^d) | Small graphs |
| DFS | No | No | O(b^m) | O(m) | Memory limited |
| UCS | Yes | Yes | O(b^d) | O(b^d) | Variable costs |
| DLS | No | No | O(b^l) | O(l) | Known depth limit |
| IDS | Yes | Yes* | O(b^d) | O(d) | Large graphs |

*When all costs are equal

---

## 5) Heuristics and Informed Search

### What is a Heuristic?

**Definition:** A heuristic is a **rule of thumb** or educated guess that helps solve a problem faster by directing search toward promising directions.

**Simple Analogy:**
- **Without heuristic:** Searching for a friend in a crowd aimlessly
- **With heuristic:** Checking heights similar to your friend first, then areas near where they usually stand

### Heuristic Function h(n)

**Definition:** h(n) = estimated cost from node n to goal

h(n) = Estimated distance to goal

Code

### Real-World Examples of Heuristics

#### 1. Map Navigation
h(n) = Straight-line (Euclidean) distance to destination

Example: Your current location: (40.7°N, 74.0°W) Destination: (51.5°N, 0.1°W)

h(n) = √[(51.5-40.7)² + (0.1-(-74.0))²] ≈ 3600 km

Code

#### 2. 8-Puzzle
h(n) = Number of tiles not in goal position

Current State: Goal State: 1 2 3 1 2 3 4 _ 5 vs 4 5 6 7 8 6 7 8 _

h(n) = 1 (only tile 6 is wrong)

Code

#### 3. Game Playing
h(n) = Evaluation of board position

Material advantage (pieces count)
Board control
King safety
Example: Chess position value = +200 favors AI

Code

### Why Heuristics are Important

1. **Reduce Search Time:** Dramatically narrows search space
2. **Guide Algorithm:** Directs focus toward goal
3. **Improve Efficiency:** Uses domain knowledge

Search Time Comparison:

Without heuristic: 1000 nodes examined With heuristic: 50 nodes examined Speedup: 20x faster

Code

### Properties of Good Heuristics

A good heuristic should be:

#### 1. **Admissible**
- Never overestimates true cost
- h(n) ≤ actual cost to goal

**Example:**
Actual distance: 100 km Admissible h(n): 100 km or less NOT admissible h(n): 150 km (overestimate)

Code

#### 2. **Consistent (Monotonic)**
- h(n) ≤ cost(n→n') + h(n')
- Guarantees optimal path

#### 3. **Informative**
- h(n) should be close to actual cost
- More informative = better algorithm performance

**Example:**
Goal distance = 50 km

Bad heuristic: h = 0 (no information) Better heuristic: h = 40 km Best heuristic: h = 49 km

Code

#### 4. **Efficient to Compute**
- Should be faster to calculate than full search

---

## 6) Informed Search Strategies

### a) Greedy Best-First Search

**How it works:**
- Always chooses node with **smallest h(n)**
- Tries to reach goal as quickly as possible

**Algorithm:**
Create priority queue sorted by h(n)
While queue not empty: a. Pop node with smallest h(n) b. If it's goal, return SUCCESS c. Generate children d. Add to queue sorted by h(n)
Code

**Example: Map Navigation**
Start: A Goal: G h(A) = 10 h(B) = 8 h(C) = 6

Greedy chooses path toward C first (lowest h)

Code

**Advantages:**
✓ Fast in many cases
✓ Low memory usage
✓ Intuitive approach

**Disadvantages:**
✗ Not complete (may get stuck in dead ends)
✗ Not optimal (may find longer path)

**When it fails:**
Code
    A (h=10)
   / \
  B   C (h=6)
  |   |
  D   E
  |
  G (Goal)
Greedy picks C (h=6) first But actually: A→B→D→G is shortest

Code

---

### b) A* Search

**Definition:** A* combines actual cost and estimated cost:

f(n) = g(n) + h(n)

Where: g(n) = actual cost from START to node n h(n) = estimated cost from node n to GOAL f(n) = total estimated cost of path through n

Code

**Visual Breakdown:**
Code
    Start
     |
     |← g(n) = 5
     |
     n
     |
     |← h(n) = 8
     |
    Goal
f(n) = 5 + 8 = 13

Code

**Algorithm:**
Create priority queue sorted by f(n)
Track visited nodes to avoid cycles
While queue not empty: a. Pop node with smallest f(n) b. If it's goal, return SUCCESS c. For each child:
Calculate g(child), h(child), f(child)
If not visited or better path found, add to queue
Code

**Complete Example:**
Map with edges: A ─5─ B ─3─ D | | 7 2 | | C ─6─ E ─4─ F

Start: A, Goal: F Heuristics: h(A)=10, h(B)=8, h(C)=9, h(D)=2, h(E)=3, h(F)=0

Step 1: Expand A f(A) = 0+10 = 10

Step 2: Expand neighbors f(B) = 5+8 = 13 f(C) = 7+9 = 16 Choose B (smallest f)

Step 3: Expand B f(D) = 8+2 = 10 Choose D (smallest f)

Step 4: Expand D f(F) = 10+0 = 10

Found F! Path: A→B→D→F, Cost: 10

Code

**Advantages:**
✓ **Complete** - always finds solution
✓ **Optimal** - finds best path (if h is admissible)
✓ **Efficient** - balances exploration with greedy guidance
✓ Most popular search algorithm

**Disadvantages:**
✗ Memory usage still high for large spaces
✗ Performance depends on heuristic quality

**When to use:**
- Need optimal solution
- Good heuristic is available
- Search space not too large
- Classic choice: pathfinding, game playing

### Comparison: Greedy vs A*

Problem: Find path from S to G

Code
     S ─1─ A
     |     |
     |     |
     5     3
     |     |
     B ─1─ G
Without heuristic: S → A → G (cost 4)

Greedy (h = straight-line distance): If h(A) = low, h(B) = high May pick: S → A → G (correct by luck)

A* (f = g + h): Evaluates g(n) too Always picks: S → A → G (cost 4) Guaranteed optimal

Code

---

## 7) Constraint Satisfaction Problems (CSP)

### Definition

A **Constraint Satisfaction Problem (CSP)** consists of:
1. **Variables** - entities we need to assign values to
2. **Domains** - possible values for each variable
3. **Constraints** - restrictions on value combinations

**General form:**
CSP = (X, D, C)

X = {X₁, X₂, ..., Xₙ} - variables D = {D₁, D₂, ..., Dₙ} - domains for each variable C = {C₁, C₂, ..., Cₘ} - constraints

Code

### Real-World CSP Examples

#### 1. Sudoku

Variables: 81 cells Domains: {1,2,3,4,5,6,7,8,9} Constraints:

Each row: all different
Each column: all different
Each 3×3 box: all different
Example partial solution: 5 3 _ | _ 7 _ | _ _ _ 6 _ _ | 1 9 5 | _ _ _ _ 9 8 | _ _ _ | _ 6 _ -------+--------+-------

Code

#### 2. Map Coloring

Variables: States {WA, NT, Q, NSW, V, SA, T} Domains: Colors {Red, Green, Blue} Constraints: Adjacent states ≠ same color

Code
    WA ── NT ── Q
    |     |     |
    SA ── NSW ──
    |     |
    V ────
Solution: WA=Red, NT=Green, Q=Red, NSW=Green, V=Red, SA=Blue, T=Blue

Code

#### 3. N-Queens Problem

Variables: 8 queens (Q₁, Q₂, ..., Q₈) Domains: Row position {1,2,...,8} for each column Constraints:

No two queens in same row
No two queens in same column
No two queens on same diagonal
Visual (4-Queens): _ Q _ _ _ _ _ Q Q _ _ _ _ _ Q _

Code

#### 4. Scheduling/Timetabling

Variables: Classes {C₁, C₂, C₃, ...} Domains: Time slots {Mon-9am, Mon-10am, ..., Fri-5pm} Constraints:

No student in two places same time
Room capacity limitations
Teacher availability
Code

### CSP Solving Methods

#### 1. **Backtracking Search**

**How it works:**
- Assign values one variable at a time
- Check constraints
- Backtrack if constraint violated

**Algorithm:**
Select unassigned variable
Try each value in domain
Check if assignment consistent with constraints
If yes, recursively assign remaining variables
If no, backtrack and try next value
Code

**Example: 4-Queens**
Step 1: Place Q₁ at row 1 Step 2: Place Q₂ at row 3 (rows 1,2 attacked) Step 3: Place Q₃ - all rows attacked by Q₁ or Q₂ Backtrack Q₂ Step 4: Try Q₂ at row 4 ... continue ...

Code

#### 2. **Constraint Propagation**

**Idea:** Remove inconsistent values before search

**Forward Checking:**
When assigning X=a:

Remove values inconsistent with a from neighbors
Reduces search space early
Code

**Example: Map Coloring**
Assign: WA = Red
      
Propagate:

NT domain: Remove Red → {Green, Blue}
SA domain: Remove Red → {Green, Blue}
Next assignment uses reduced domains

Code

#### 3. **Heuristics for CSP**

**a) Minimum Remaining Values (MRV)**
Choose variable with smallest domain first Example: If V has 2 values, assign V before W (5 values)

Code

**b) Degree Heuristic**
Choose variable involved in most constraints

Code

**c) Least Constraining Value**
Choose value that eliminates fewest options for neighbors

Code

### Why CSP Matters

Many real-world problems are CSPs:
- **Scheduling** (school, work, transport)
- **Resource allocation** (rooms, teachers, equipment)
- **Design problems** (circuit layout, building architecture)
- **Games and puzzles** (Sudoku, crosswords)
- **Planning** (action sequences)

### CSP Complexity

Complexity grows exponentially with:

Number of variables
Domain size
Number of constraints
Example: 4-Queens

Variables: 4
Domain size: 4
Possible combinations: 4⁴ = 256
8-Queens

Variables: 8
Domain size: 8
Possible combinations: 8⁸ = 16.7 million (But only 92 actual solutions!)
Code

---

## Summary Table: Unit I Concepts

| Concept | Definition | Key Example |
|---------|-----------|------------|
| **AI** | Field creating intelligent machines | Medical diagnosis |
| **Agent** | System that perceives and acts | Robot vacuum |
| **Rational Agent** | Acts to maximize utility | Choosing job offer |
| **Problem Formulation** | Defining initial, goal, actions | Map navigation |
| **BFS** | Queue-based level exploration | Finding shortest path |
| **DFS** | Stack-based deep exploration | Memory-limited search |
| **UCS** | Expands lowest cost nodes | Shortest distance path |
| **Heuristic** | Estimated cost to goal | Straight-line distance |
| **Greedy Search** | Follows heuristic only | Fast but not optimal |
| **A* Search** | f(n)=g(n)+h(n), optimal | GPS navigation |
| **CSP** | Variables, domains, constraints | Sudoku puzzle |

