<h1>LuxPilot Case Study</h1>

<h2>Summary</h2>

<p>
  LuxPilot is a governed LLM advisory system for small business decision support.
  This case study documents the design decisions that keep the system
  deterministic, auditable, model-agnostic, and evaluation-ready.
</p>

<p>
  The repository shares a sanitized public version of the architecture,
  synthetic fixtures, toy examples, public schemas, and demonstration
  screenshots. It does not disclose production rules, exact gate predicates,
  production prompts, commercial templates, real user data, or real execution
  logs.
</p>

<h2>The problem</h2>

<p>
  Advisory and decision-support products are tempting to build entirely on top
  of an LLM. The model can ask questions, interpret user input, generate advice,
  and explain itself fluently.
</p>

<p>
  But when the LLM sits on the decision path, three hard problems appear:
</p>

<ol>
  <li>
    <strong>Non-determinism.</strong> The same input can produce different
    recommendations.
  </li>
  <li>
    <strong>Non-reproducibility.</strong> It becomes difficult to replay why a
    decision was made.
  </li>
  <li>
    <strong>Opaque accountability.</strong> When a recommendation is wrong, it
    is unclear whether the failure came from the input, the rule logic, the
    prompt, or the model.
  </li>
</ol>

<p>
  For an advisory product, these are not edge cases. They are the core
  governance risk.
</p>

<p>
  A fluent answer is not enough. The system must also be feasible, inspectable,
  replayable, and safe for human validation.
</p>

<h2>The core design decision</h2>

<p>
  LuxPilot separates <strong>deciding</strong> from
  <strong>communicating</strong>.
</p>

<ul>
  <li>
    <strong>Deciding</strong> is handled by a deterministic routing engine.
  </li>
  <li>
    <strong>Communicating</strong> is handled by the LLM.
  </li>
</ul>

<p>
  The LLM may extract candidate variables, explain a decision, generate draft
  assets, or ask adaptive questions. It does not choose the strategic outcome.
</p>

<p>This is the central governance boundary of LuxPilot:</p>

<pre><code>confirmed snapshot -&gt; deterministic routing -&gt; structured result
LLM output -&gt; explanation / candidate / draft -&gt; user validation</code></pre>

<h2>System approach</h2>

<p>LuxPilot is structured around three layers.</p>

<h3>Layer 1 — Versioned domain knowledge</h3>

<p>
  The production system uses structured domain knowledge: profile variables,
  intervention families, feasibility concepts, templates, language rules, and
  explanatory metadata.
</p>

<p>
  In this public repository, production rule content is intentionally excluded.
  Only sanitized schemas, synthetic fixtures, and toy examples are included.
</p>

<h3>Layer 2 — Deterministic governance engine</h3>

<p>
  The deterministic engine consumes a confirmed input snapshot and a versioned
  rulebase.
</p>

<p>
  For the same snapshot, same rulebase version, and same engine build, the
  system should produce the same routing result.
</p>

<p>
  The engine does not call an LLM, does not use randomness, does not depend on
  model providers, and does not generate free-form prose.
</p>

<h3>Layer 3 — Model-agnostic LLM execution</h3>

<p>The LLM layer is used for bounded execution tasks:</p>

<ul>
  <li>extraction;</li>
  <li>inference;</li>
  <li>boundary interpretation;</li>
  <li>explanation;</li>
  <li>generation;</li>
  <li>adaptive questioning.</li>
</ul>

<p>
  These functions are intended to run behind JSON contracts so that model
  providers can be swapped without changing the decision logic.
</p>

<h2>What this design enables</h2>

<p>The architecture gives LuxPilot several useful properties.</p>

<h3>Determinism</h3>

<p>Identical confirmed inputs can produce identical routing results.</p>

<h3>Replayability</h3>

<p>A routing decision can be tied to a replay contract:</p>

<pre><code>snapshot_hash + rulebase_version + engine_build -&gt; output_hash</code></pre>

<h3>Model-agnosticism</h3>

<p>
  The model can be changed behind function contracts without changing the
  deterministic routing decision.
</p>

<h3>Clearer accountability</h3>

<p>
  Input interpretation, routing logic, model generation, and user validation are
  separated into different layers.
</p>

<p>This makes it easier to identify whether a failure came from:</p>

<ul>
  <li>bad or incomplete input;</li>
  <li>an incorrect assumption;</li>
  <li>the deterministic rule layer;</li>
  <li>the explanation layer;</li>
  <li>the generation layer;</li>
  <li>missing user validation.</li>
</ul>

<h3>Evaluation readiness</h3>

<p>
  The system is designed so that behavior can be tested and inspected through
  synthetic fixtures, public schemas, audit examples, and human-readable
  governance tests.
</p>

<h2>Visual walkthrough</h2>

<p>
  A visual walkthrough of the public demo flow is available in
  <a href="docs/screenshots/">docs/screenshots</a>.
</p>

<p>Key screens include:</p>

<ul>
  <li>landing page and value proposition;</li>
  <li>offering-line confirmation;</li>
  <li>focus and challenge selection;</li>
  <li>routed strategy recommendation;</li>
  <li>capacity-aware action plan.</li>
</ul>

<p>
  The screenshots use synthetic/demo data and do not disclose production rules,
  prompts, templates, real user profiles, or execution logs.
</p>

<h2>Synthetic walkthrough</h2>

<p>
  This public walkthrough uses a synthetic example. It does not represent
  production rule logic.
</p>

<h3>Example user</h3>

<p>
  A small service business wants more customers but has limited weekly capacity
  and weak visible trust assets.
</p>

<h3>Step 1 — Intake</h3>

<p>
  The user describes the business or provides a website link.
</p>

<p>
  The LLM may help extract candidate variables, but those variables are not
  automatically trusted.
</p>

<h3>Step 2 — Confirmation</h3>

<p>
  The system asks the user to confirm or correct interpreted profile elements
  such as:
</p>

<ul>
  <li>buyer type;</li>
  <li>offering lines;</li>
  <li>product/service type;</li>
  <li>geography;</li>
  <li>trust assets;</li>
  <li>platform dependency;</li>
  <li>current marketing challenge.</li>
</ul>

<p>Unconfirmed LLM extraction cannot enter routing.</p>

<h3>Step 3 — Snapshot</h3>

<p>
  Once the relevant variables are confirmed, the system freezes them into a
  profile snapshot.
</p>

<p>
  That snapshot becomes the accountable input to deterministic routing.
</p>

<h3>Step 4 — Routing</h3>

<p>The deterministic engine receives:</p>

<pre><code>confirmed snapshot + rulebase version</code></pre>

<p>
  It returns a structured recommendation, trace summary, pre-steps if needed,
  and a routing result.
</p>

<p>The LLM does not choose this recommendation.</p>

<h3>Step 5 — Explanation</h3>

<p>
  The LLM may explain the result in plain language, but it explains from the
  structured trace.
</p>

<p>
  It does not receive the production rulebase and does not re-route the user.
</p>

<h3>Step 6 — Action plan</h3>

<p>
  The system generates a bounded action plan inside the routed recommendation.
</p>

<p>
  The plan is designed to remain feasible for the user’s available capacity.
</p>

<p>
  Generated assets remain drafts until reviewed.
</p>

<h3>Step 7 — Validation</h3>

<p>
  The user can approve, edit, reject, or ask to adjust the recommendation.
</p>

<p>
  Validation events are part of the governance loop.
</p>

<h2>What is in this repository</h2>

<p>This public case study includes:</p>

<ul>
  <li>system architecture;</li>
  <li>public specification;</li>
  <li>reproducibility design;</li>
  <li>evaluation plan;</li>
  <li>synthetic fixtures;</li>
  <li>public JSON schemas;</li>
  <li>toy rulebase;</li>
  <li>human-readable governance tests;</li>
  <li>sample audit log;</li>
  <li>replay contract;</li>
  <li>prompt hygiene example;</li>
  <li>demo screenshots.</li>
</ul>

<p>See <a href="README.md">README.md</a> for the full repository map.</p>

<h2>What is intentionally not here</h2>

<p>This repository does not include:</p>

<ul>
  <li>production code;</li>
  <li>production rulebase;</li>
  <li>exact gate predicates;</li>
  <li>exact thresholds;</li>
  <li>production prompts;</li>
  <li>commercial templates;</li>
  <li>real customer data;</li>
  <li>real user profiles;</li>
  <li>real execution logs;</li>
  <li>real golden traces;</li>
  <li>credentials or API keys;</li>
  <li>commercial strategy or pricing logic.</li>
</ul>

<p>
  See <a href="REDACTION_POLICY.md">REDACTION_POLICY.md</a>,
  <a href="PUBLIC_ARTIFACT_BOUNDARY.md">PUBLIC_ARTIFACT_BOUNDARY.md</a>, and
  <a href="DISCLAIMER.md">DISCLAIMER.md</a>.
</p>

<h2>Why this matters</h2>

<p>
  LuxPilot is not presented here as a generic chatbot or a prompt wrapper.
</p>

<p>
  The point of the case study is to show how an LLM advisory system can be
  structured so that:
</p>

<ul>
  <li>the model is useful but bounded;</li>
  <li>strategic decisions are deterministic;</li>
  <li>user inputs are confirmation-gated;</li>
  <li>recommendations are traceable;</li>
  <li>outputs can be validated;</li>
  <li>decisions can be replayed;</li>
  <li>evaluation is designed into the system rather than added later as decoration.</li>
</ul>

<p>The broader lesson is simple:</p>

<pre><code>Good AI advisory systems should not only sound intelligent.
They should be governable.</code></pre>
