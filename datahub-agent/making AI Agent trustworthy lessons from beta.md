---
title: making AI Agent trustworthy lessons from beta
url: https://datahub.com/blog/making-ai-agents-trustworthy-lessons-from-beta/
created: 2026-10-29
---

Many teams building data analytics agents start with the assumption that building a production-grade agent requires little more than connecting the latest and greatest AI model to their data warehouse. We spent five months in the private beta for DataHub Cloud Context with enterprise customers across financial services, retail, and technology, and that assumption proved wrong time and time again. What ultimately determined whether an analytics agent earned organizational trust was the business context it had access to, the discipline a team brought to maintaining that context, and a deliberate focus on keeping that context high quality over time.

Based on our experience, analytics agents pointed at enterprise data warehouses without business context typically start at accuracy rates of 40% or lower when answering benchmark questions. At Miro, their Snowflake Cortex agent was delivering roughly that level of accuracy before they layered in business context provided by DataHub. After integration, accuracy climbed to 90%.

> “Starting with Snowflake metadata alone, our analytics agent answered about half of our benchmark questions correctly. After layering in DataHub as our context platform, we nearly doubled accuracy.”
> 
> 
> Miro
> 

That jump didn’t come from a better model; **it came from giving the agent the right semantic definitions, governance metadata, and institutional knowledge about how Miro’s data is used.**

## The teams that struggled had one thing in common

The clearest pattern across our beta was organizational, not technical. The teams that struggled were the ones that had skipped the groundwork. They had no clear rollout strategy, no defined roles and responsibilities, and no plan for tackling their data estate domain by domain. They tried to expose their agents to their entire data estate at once, sometimes tens of thousands of tables, and got much worse results than those who started with a narrow, well-curated slice. The sheer breadth introduced conflicting metric definitions, low quality data sources, ambiguous column names, and outdated documentation that overwhelmed the model *and* the data team trying to improve it.

Independent research points the same way. In a 2026 BARC survey of 285 data, AI, IT, and business stakeholders, organizations with the most mature context programs were far more likely to have business owners involved: 75% have business managers overseeing or executing context work, compared with 57% elsewhere. BARC’s guidance echoes what we saw in beta: success comes from “a sequence of focused, controlled wins rather than one large rollout.” The fastest teams picked a single business domain, assigned clear accountability for context creation and validation, and limited the agent’s view to that domain’s data and context before expanding.

## How evals went from unfamiliar to indispensable

When we started the beta, most teams had never built test question sets, also known as *evals*, for their analytics agents. Nor did they understand their importance in establishing a baseline and then improving agent accuracy.  Even we were guilty of initially treating evals as a second-class-citizen; it cost us weeks of toil reviewing raw context changes without a way to measure their impact.

Once teams saw that evals gave them a measurable starting point, and a concrete way to illustrate the value of context creation and curation, they became the starting point for the entire process of context engineering. Without evals, teams are unable to tell whether the context you provide to an agent is meaningfully improving its answers. But with them, every context change becomes a validatable hypothesis rather than a guess.

And the industry is catching on. Evals are not just for understanding where you start; they are the keystone for maintaining quality in agent responses over time as your business evolves, catching regressions before they reach end users when a new business process is created, a metric definition changes, or an important dataset gains new meaning.

## Text-to-SQL as the entry point, not the destination

Text-to-SQL, or agentic data analytics, is the first use case most teams pursue with AI, and it remains the most common starting point for good reason. Data teams have spent years fielding ad hoc questions, building one-off reports, and manually translating business requests into queries. An analytics agent that can reliably handle that work is the fastest path to tangible impact, and can save teams time immediately.

Pinterest’s engineering team demonstrated the opportunity of agentic data analytics at scale. With more than 100,000 analytical tables and 2,500 analysts, they found that pointing an agent directly at the warehouse wasn’t enough. The agent needed context: what data exists, which data matters, and how to use it to answer the business’s most critical questions. Capturing that context was its own challenge, so they turned to years of queries written by their best analysts. Those join patterns, filter logic, and aggregation choices encoded institutional expertise that no documentation project could replicate. Pinterest made that signal searchable for the agent and built a self-reinforcing loop in which every new analyst query added to the context. The result was an agent grounded in the questions analysts were already answering, and Pinterest’s most widely adopted AI agent by far.

Over time, data teams will evolve beyond basic question-and-answer workflows to fundamentally different ways of working. Optiver was already building toward something more ambitious: data-backed research and development where agents don’t just query existing tables but actively enrich, join, transform, and filter source data to answer specific business questions on demand. Other teams pursued data development workflows, where agents help engineers build and validate pipelines, and operational data monitoring, where agents watch for anomalies and surface issues before humans notice them.

Text-to-SQL and analytics automation are where teams first see how dramatically context improves agent quality. But the fuller vision of agentic data management extends much beyond text-to-SQL and looks much closer to what Optiver is building: dynamic teams of data agents that assemble the pipelines needed to answer today’s most important questions, then tear them down and rebuild for tomorrow’s. That shift from pre-built golden datasets to on-demand data assembly, based on an always-current context layer,  is already underway.

## A playbook from the teams that moved the fastest

None of that future is reachable unless the first agent earns trust. Across our beta, the teams that got there fastest followed a repeatable sequence that treats context as a first-class citizen from day one.

1. **Start with a narrow domain.** Pick a specific business area, such as sales reporting for board metrics or product analytics for your PM team, and start by exposing that domain’s tables and context to the agent. Build a tailored set of eval questions that covers the most critical questions one may have about that domain to build confidence. Launch to production with real end users. And then repeat the process across more domains.
2. **Build evals *before* spending time on context.** An administrator should work with a subject-matter expert to define test questions and expected answers for their domain. Without evals, you have no baseline and no way to measure whether context helps or hurts.
3. **Think about context in layers.** Start with the technical layer, a raw inventory of tables, columns, dashboards, and data processing jobs, plus query history and other operational signals. Then move to the semantic and metrics layer, which contains explicit definitions that guide agents on how to compute tier-one metrics consistently and correctly. Most often, we saw customers using tools like dbt, Cube, or LookML for these definitions. Finish with the knowledge layer, unstructured guidance and explicit tribal knowledge that helps agents understand why certain data exists, key procedures and caveats in using the data, and how to drive specific business outcomes. DataHub enables data teams to ingest all three, then infers context to fill gaps for the long tail of tier-two data that nobody has had time to hand-curate.
4. **Validate continuously.** Continuously validate context quality through both evals and monitoring of agents in production. Evals help you verify that the context you’ve aggregated is able to answer your eval test questions, while live feedback lets agents proactively report cases where they used missing, conflicting, or incorrect context when answering user questions or completing tasks.

A context engineering project builds context for a single agent and moves on. Context management treats context as a living system, and the teams that understand this are actively creating the processes and structure necessary for them to build evals, maintain context, and monitor agents in production at scale. During the beta, we saw motivated teams completing the full journey for a given data domain, from evals to context to production to feedback in a matter of weeks.

## Where human expertise has the highest leverage

We went into beta convinced that human-in-the-loop context review would be essential, and it is, though not where we expected. Our initial product experience started with review workflows where experts would manually review AI-generated context. Almost nobody wanted to do that, especially given the volume of rich semantic context that existing data tools can generate from the available exhaust (queries, dashboard definitions, and so on). There was no way to demonstrate the impact of these review efforts without evals.

We found that human expertise has the highest leverage at two specific points in the process:

1. **Defining the eval question corpus.** A product analytics manager who spends thirty minutes defining ten test questions contributes more to agent accuracy than weeks of manual documentation may.
2. **Reviewing feedback from agents running in production.** When an agent encounters missing, conflicting, or potentially incorrect context during a real conversation, it can proactively report that gap, surfacing it for review by central platform teams.

Based on these learnings, we shifted our approach so that auto-generated context is published by default, and evals serve as a quality gate. And once agents are in production, with access to this context, they can report feedback back into DataHub, where admins can review it.. As a result, if a finance analyst reports an incorrect revenue definition on Tuesday, context admins can correct it so that the same question asked again on Thursday is grounded in authoritative context. Accuracy compounds with use instead of decaying over time.

## The strategic value of a centralized context layer

We went into the beta with a thesis we wanted to test: that there is strategic value in maintaining an independent and authoritative context layer. Context has traditionally been scattered across tools or locked in people’s heads, and agents can only ground their answers in context they can reach. While each data tool in an enterprise stack provides valuable context within its own ecosystem, the most critical use cases across data analytics, governance, and quality demand more: a single, bird’s-eye view that connects context across siloed systems and prevents context fragmentation.

A context layer that sees only one platform’s world will be incomplete for any organization with a heterogeneous data estate. DataHub sits across it all, with connectors to more than 150 data sources, and delivers technical, semantic, and knowledge layer context through MCP, API, and SDK to any agent, whether that is Claude, ChatGPT, Snowflake Cortex, Databricks Genie, Cursor, or a custom agent.

## Where we’re going next

By the end of the beta, the question we heard most often had shifted from whether context matters to how to build an organization that manages context well. Not for a single agent or a single quarter, but as a continuous discipline that evolves alongside the data estate.

We’ve seen this pattern before. Data engineering became a discipline when pipelines outgrew ad hoc scripts, and data quality management became a formal practice once the cost of bad data became measurable. Context management is following the same trajectory. Agents running on unmanaged context produce unreliable outputs at a scale and speed that’s impossible to overlook, and the organizations responding best are treating context as its own product within the data organization.

That shift brings new roles. Context engineers own the infrastructure that ingests, enriches, and delivers context to agents. Context stewards, typically domain experts embedded in business units, own the correctness of context within their domains, much as data stewards own data quality. Both work with a set of context primitives that make the discipline sustainable rather than heroic: context documents that capture definitions and institutional knowledge, evals that test context quality and catch regressions when definitions or schemas change, live feedback that surfaces missing or ambiguous context as agents encounter it in real conversations, and context agents that keep context correct with minimal human touch as schemas evolve, metrics are redefined, and new sources come online.

Everything we’re bringing to GA reflects what five months of beta taught us, from how evals, domains, and context documents work to how scoped MCP servers deliver context to agents. DataHub’s role is to be the foundation for a domain-specific context management strategy, one that spans the use cases where context-driven AI already delivers measurable value (text-to-SQL analytics, data development, data quality management, and governance) and is built to support the use cases that come next.

*John Joyce is co-founder and head of product at DataHub, where he leads the development of the context platform that helps enterprises make their data trustworthy for AI agents. Before co-founding DataHub, he built data infrastructure at LinkedIn.*