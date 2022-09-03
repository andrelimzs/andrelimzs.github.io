---
title: "Reinforcement Learning"
date: 2022-04-08T23:59:00+08:00
draft: false
ShowToc: true
math: true
---

## Reinforcement Learning

Many sequential decision making and control problems are hard to provide explicit supervision for.

Instead provide a reward function and let the learning algorithm figure out how to choose actions over time.

Successful in applications such as helicopter flight, legged locomotion, network routing, marketing strategy selection, etc

## Markov Decision Processes (MDP)

Provide formalism for many RL problems

### Terminology

- **States** : $s$
- **Actions** : $a$
- **State Transition Probabilities** : $P_{sa}$
- **Discount Factor** : $\gamma \in [0,1)$
- **Reward Function** : $R : S \times A \mapsto \mathbb{R}$

### Dynamics of MDPs

1. Start in some state $s_0$
2. Choose some action $a_0 \in A$
3. MDP randomly transitions to successor state $s_1 \sim P_{s_0a_0}$
4. Pick another action
5. Repeat

Represent as
$$
s_0 
\overset{a_0}{\rightarrow} s_1
\overset{a_1}{\rightarrow} s_2
\overset{a_2}{\rightarrow} s_3
\overset{a_3}{\rightarrow} \dots
$$
**Total Payoff**
$$
R(s_0, a_0) + \gamma R(s_1, a_1) + \gamma^2 R(s_2, a_2) + \dots
$$
**Goal** : Maximise expected value of total payoff
$$
E \left[
R(s_0) + \gamma R(s_1) + \gamma^2 R(s_2) + \dots
\right]
$$

### Policy

Function $\pi : S \mapsto A$ maps states to actions

**Execute** a policy by following the prescribed action

### Value Function

The expected sum of discounted rewards upon starting in $s$ and taking actions according to $\pi$
$$
V^\pi(s) = \mathbb{E}
\left[
    R(s_0) + \gamma R(s_1) + \gamma^2 R(s_2) +
    \dots | s_0 = s,\pi
\right]
$$

### Bellman Equation

Given a fixed policy $\pi$, it's value function $V^\pi$ satisfies the Bellman equations
$$
V^\pi(s) = R(s) + \gamma \sum_{s' \in S} P_{s\pi(s)}(s') V^\pi(s')
$$
Which consists of two terms:

- Immediate reward $R(s)$
  Get for starting in $s$
- Expected sum of future discounted rewards
  Can we rewritten as $E_{s'\sim P_{s\pi(s)}}[V^\pi(s')]$
  The expected sum of starting in $s'$ where $s'$ is distributed according to to $P_{s\pi(s)}$

Can be used to efficiently solve for the value function $V^\pi$

- Write down one equation for every state
- Gives a set of $|S|$ linear equations in $|S|$ variables

### Optimal Value Function

Best possible expected sum of discounted rewards which can be attained using any policy

$$
V^*(s) = \max_\pi V^\pi (s)
$$

Which has it's own version of the **Bellman Equation**

$$
V^{\ast}(s) = R(s) + \max_{a\in A} \gamma
\sum_{s'\in S}{P_{sa} (s') V^{\ast}(s')}
$$

The second term is a $\max$ over all possible actions because that is the optimal reward.

### Optimal Policy

$$
\pi^{\ast}(s) = \arg \max_{a\in A} \sum_{s'\in S} P_{sa}(s') V^*(s')
$$

Which gives the action that attains the $\max$ in the optimal value function

For every state $s$ and policy $\pi$
$$
V^\ast(s) = V^{\pi*}(s) \geq V^\pi(s)
$$
Which says that

> The value function for the optimal policy is equal to the optimal value function for every state $s$

> The value function for the optimal policy is greater than or equal to every other policy

- $\pi^*$ is the optimal policy for **all** states



## Value Iteration and Policy Iteration

Two efficient algorithms for solving finite-state MDPs with known transition probabilities

### Value Iteration

> For each state s, initialize $V(s) := 0$
> for until convergence do
> 	For every state, update
> $$
> V(s) := R(s) + \max_{a\in A} \gamma
> \sum_{s'} P_{sa}(s') V(s')
> $$

- Repeatedly update estimated value function using the Bellman equation

Can be:

- Synchronous
  - Compute new values for **all** states and update at once
  - Can be viewed as implementing a *Bellman Backup Operator*, which maps the current estimate to a new estimate
- Asynchronous
  - Loop over states and update one at a time

Both algorithms will cause $V$ to converge to $V^*$

### Policy Iteration

>Initialise $\pi$ randomly
>
>for until convergence do
>
>​	Let $V := V^\pi$
>
>​	For each state $s$
>$$
>\pi(s) := \arg \max_{a \in A} \sum_{s'} P_{sa}(s') V(s')
>$$

- Repeatedly computes value function for the current policy
- Update policy using current value function
  - This is the policy that is **greedy w.r.t. $\bold V$**
- The value function can be updated using Bellman equations
- After a finite number of iterations, $V$ will converge to $V^\ast$ and $\pi$ to $\pi^\ast$

### Value vs Policy Iteration

- No universal agreement which is better
- Small MDPs : Policy iteration tends to be fast
- Larger MDPs : Value iteration might be faster
- Policy iteration can reach the exact $V^*$
- Value iteration will have small non-zero error



## Learning a Model for an MDP

- In many realistic problems, we do not know the state transition probabilities and rewards

- Must be estimated from data

Given enough trials, can derive **maximum likelihood estimates** for the state transition probabilities
$$
P_{sa}(s') =
\frac{num. \text{ times } a \text{ led to } s' }
{num. \text{ times we took } a}
$$
In the case of $\frac{0}{0}$, can set $P$ to $\frac{1}{|S|}$ (estimate as uniform)

### Efficient Update

Keep count of numerator & denominator

### Solving Learned MDP

- Can use **policy**/**value** iteration



## Continuous State MDPs

- Infinite number of states
- Consider settings where state space is $S = \mathbb{R}^d$

### Discretization

- Simplest way
- Discretize state space
- Use value/policy iteration

**Downsides**

- Fairly naive representation for $V^*$
- Assumes the value function is constant over each discretization interval
- Piecewise constant representation isn't good for many smooth functions
  - Little smoothing over inputs
  - No generalization over different grid cells
- Suffers from **curse of dimensionality**
  - For $S = \mathbb{R}^d$, we will have $k^d$ states

**Rule of Thumb**

- Works well for 1D/2D cases
- Can work for 3D/4D cases
- Rarely works above 6D



### Value Function Approximation

- Approximate $V^*$ directly

### Using Model/Simulator

- Assume we have a model/simulator for the MDP
- Any black-box with
  - Input : State $s_t$, Action $a_t$
  - Output : Next-state $s_{t+1}$ sampled

### Getting the Model

#### Using physics simulation

- Can use off-the-shelf physics simulation

#### Data collected in the MDP

- Can be done with:
  - Random actions
  - Executing some policy
  - Other way of choosing actions \
    [?] Control law?
- Apply learning algorithm to predict $s_{t+1}$ as function of $s_t$

**Example** : Learn linear model

using linear regression
$$
\arg \min_{A,B} \sum_{i=1}^{n} \sum_{t=0}^{T-1}
\left\Vert
	s_{t+1}^{(i)} - (A s_t^{(i)} + Ba_t^{(i)})
\right\Vert^2_2
$$
Can build either a *deterministic*
$$
s_{t+1} = As_t + Ba_t
$$
or *stochastic* model
$$
s_{t+1} = As_t + Ba_t + \epsilon_t
$$
with $\epsilon_t$ being a noise term usually modelled as $\epsilon_t \sim \mathcal{N}(0,\Sigma)$

or non-linear function
$$
s_{t+1} = A\phi_s(s_t) + B\phi_a(a_t)
$$
Eg: Locally weighted linear regression



### Fitted Value Iteration

- Algorithm for approximating the value function of a continuous state MDP
- Assume state space $S = \mathcal{R}^d$
- But action space $A$ is **small** and **discrete**

Perform value iteration update
$$
\begin{aligned}
	V(s) :&= R(s) + \gamma \max_a
	\int_{s'} P_{sa}(s') V(s') ds' \cr
	&= R(s) + \gamma \max_a E_{s' \sim P_{sa}} [V(s')]
\end{aligned}
$$

- Main difference is the $\int$ instead of $\sum$
- Approximately update over a finite sample of states $s^{(1)}, \dots, s^{(n)}$
- Use supervised learning algorithm, **linear regression** to approximate value function as linear/nonlinear function of the states

$$
V(s) = \theta^T \phi(s)
$$

**Algorithm**

> Compute $y^{(i)}$, the approximation to $R(s) + \gamma \max_a E_{s' \sim P_{sa}} [V(s')]$ (RHS)
>
> Use supervised learning to get $V(s) = y^{(i)}$
>
> Repeat

#### Convergence

- Cannot be proved to converge
- But in practice it often does, and works well for many problems

#### Deterministic Simplification

- Can set $k=1$
- Because expectation of a deterministic distribution requires 1 sample

#### Policy

- Outputs $V$, an approximation of $V^*$
- Implicitly defines policy

When in state $s$, choose
$$
\arg \max_a E_{s'\sim P_{sa}} [V(s')]
$$

- Process for computing/approximating this is similar to the fitted value iteration algorithm

