# ai-architecture

Thoughts on patterns and concepts related to stochastic distributed systems.

<!-- vim-markdown-toc GFM -->

- [An Ontology of Agentic and Procedural Systems](#an-ontology-of-agentic-and-procedural-systems)
    - [Stochastic Distributed Systems](#stochastic-distributed-systems)
    - [Agentic and Procedural Systems](#agentic-and-procedural-systems)
        - [Comparing Agentic and Procedural Systems: A Real-World Example](#comparing-agentic-and-procedural-systems-a-real-world-example)
        - [Agency is **Not** the Ability to Take Action](#agency-is-not-the-ability-to-take-action)
    - [Agents](#agents)

<!-- vim-markdown-toc -->

## An Ontology of Agentic and Procedural Systems

Much has been written about Agentic Systems. Over time we will likely converge on a common terminology and understanding of the key concepts. However, I have found that there is currently significant differences in how agentic and non-agentic systems are described.

Often when talking about agentic systems, they are compared to "non-agentic" systems. A better term than "non-agentic" system could be "procedural", which implies a system that is designed in a more deterministic fashion.

A simple ontology for "Agentic" and "Procedural" systems is below; feedback and comments would be welcome. We build mental models to help us reason more effectively - this is a model that I have found useful. I would also welcome feedback and comments.

### Stochastic Distributed Systems

A "distributed" system is a technology system that has compute and data spread across multiple nodes. Many, of not most, non-trivial IT systems are distributed, across many database servers, application servers, APIs and so on.

A "stochastic distributed system" is a distributed system that contains components that are by-design probabalistic in nature. An example of such a component would be an "Agentic System", such as a system that can perform actions based on the outputs of a large-language model.

To reason about stochastic distributed systems it is essential to create an ontology that describes an agentic and non-agentic system.

### Agentic and Procedural Systems

Conceptually, agentic and procedural systems can be defined like so:

An **Agentic System** is a system that is _authorised_ to perform _tasks_, given a set of _permissions_, _tools_ and _instructions_, with the _actions_ it takes being **determined by** a _stochastic model_ (such as an LLM).

A **Procedural System** is a system that is _authorised_ to perform _tasks_ given a set of _permissions_, _tools_ and _instructions_, with the _actions_ it takes being **determined by** a _procedural model_ (such as a set of rules).

Whilst elements of AI are highly novel, agentic systems are actually strikingly similar to procedural systems:

- Authorisation: we authorise systems to take actions based on various parameters
- Tasks: systems are given 'units of work' to perform, with specific inputs leading to actions being taken
- Permissions: systems are given access to various resources, with well-defined constraints and limitations
- Tools: systems can be given the ability to interface with _other_ systems via various interfaces
- Instructions: systems are designed to follow a set of rules, policies or guidelines
- Actions: systems complete tasks by performing one or many operations
- Determination: systems are designed to select an action based on a 'model' - which is either:
   - Stochastic: a model that is non-deterministic (or probabalistic) in nature
   - Procedural: a model that is deterministic in nature

As we will see in the comparison which follows, the fundamental difference between an agentic and procedural system is simply down to whether the underlying model is deterministic or non-deterministic. The consequences of this difference however are huge.

Note that whilst the model that makes up a procedural system (such a set of rules or DAG) is deterministic, the _implementation_ of the system and its execution is not, as there are always 'real world' probabalistic factors that are not modeled (such as network partitions, hardware failures, interference and so on).

#### Comparing Agentic and Procedural Systems: A Real-World Example

**A Procedural Loan-Approval System**

Consider a "Loan Approval" system. The goal of this system is to take a set of input parameters, such as the details of an account holder, the parameters of the loan they are applying for, and perform an action - either approving, denying, or conditionally approving a loan.

A procedural system would likely the best choice in most cases to define such a system. This is because the model for the system can often be quite simple; it might be something like "if the applicant receives 5x the loan amount per-year as their salary and has not defaulted on any loan payments, we will approve. If they receive 2-5x the amount, we conditionally approve but require manual checks, if less than 2x we deny.

The model in this case is procedural in that it follows a well-defined and deterministic sequence of steps - the system is operating based on a design. It is possible that a system like this may have some statistical elements, but at its core the design is deterministic - we expect to be able to determine the output consistently given the input. This is in fact essential for most loan-approval systems as the process by which the determination is made must be explainable.

**An Agentic Busy-Day Alerting System**

Now consider a "busy day alerting" system. The goal of this system is to monitor a users email inbox and calendar and warn them in advance if it appears that a day in the future may have too many events or activities going on.

Historically, a system like this might have been procedural, made up of many complex and hard-coded rules. This system would be "brittle" in that it would depend upon logic to try and determine from the text of an email whether an event is being planned for a specific day.

A stochastic model such as an LLM could instead be used to create an agentic system that looks through the contents of a users emails, their calendar, and make predictions of what events _might_ be being planned for a given set of days.

In this case, the agentic system is designed to be non-deterministic - we don't need to be able to follow the reasoning of the system, and in fact we don't want to - the problem is one that is complex enough that designing a set of rules to make a determination is far too complex (and therefore costly to implement and maintain).

#### Agency is **Not** the Ability to Take Action

When compared to an LLM, the "agency" in an agentic system can indeed be defined as the ability to take actions.

However, when the ontology of agentic and procedural systems defined above is used, this is not the case.

Both the procedural loan-approval system and the agentic busy-day alerting system can take actions. They both have authority to take actions, use tools (such as APIs or database clients to perform queries), they both take actions (such as changing the state of a 'loan request' entity in a system, or sending a 'busy day' alert).

The "Agency" in the agentic systems as defined above is as follows:

- **Agency** in **Agentic System** is the authority to determine a set of actions to take to complete a task, using a **non-deterministic model**.

### Agents

Given the ontology above, it is also straightforward to define what an 'agent' is. It is simply an agentic system:

An **Agent** is a system that is _authorised_ to perform _tasks_, given a set of _permissions_, _tools_ and _instructions_, with the _actions_ it takes being **determined by** a _stochastic model_ (such as an LLM).

In many examples the 'authorisation', 'tasks' and 'permissions' are not explicitly called out, and an agentic might look like this:

```
# Busy Day Agent

Instructions: you are a helpful assistant that looks through my emails and calendar and alerts me if there are upcoming days that might be busy enough that I need to change their arrangements.

Tools:
- Read email tool
- Open calendar tool
- Send Alert tool
```

When we run an agent, we are authorising it to perform the task of checking for busy days, given the instructions and using its tools. The permissions have not been called out in this case, but are (hopefully) still in the designed system in that it should be able to read the users emails, and not the emails of others, alert the user (but not someone else) and so on.

---

**Disclaimer**

The opinions, statements and ideas in this repository are my own and do not represent the views of my employers. I work professionally in software engineering, architecture and AI, and this repository contains ideas I am playing around with, exploring and seeking input on.
