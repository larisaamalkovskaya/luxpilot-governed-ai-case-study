<h1>LuxPilot — Governed LLM Advisory System</h1>

<blockquote>
  <p>
    Public case study of <strong>LuxPilot</strong>: a governed LLM advisory
    system that separates deterministic routing from probabilistic generation.
  </p>
</blockquote>

<p>LuxPilot is designed around one core boundary:</p>

<pre><code>confirmed snapshot -&gt; deterministic routing -&gt; structured result
LLM output -&gt; explanation / candidate / draft -&gt; user validation</code></pre>

<p>
  This repository is a <strong>public, sanitized case-study artifact</strong>.
  It is not the production LuxPilot codebase.
</p>

<p>
  It contains architecture documentation, public schemas, synthetic fixtures,
  toy examples, evaluation design, prompt-hygiene examples, audit/replay
  concepts, and public demo screenshots. It intentionally excludes production
  rules, exact gate predicates, production prompts, commercial templates, user
  data, real execution logs, and real golden traces.
</p>

<h2>Demo</h2>

<ul>
  <li>Live demo: <a href="https://luxpilot.app">https://luxpilot.app</a></li>
  <li>Portfolio page: <a href="https://larisaamalkovskaya.github.io/">https://larisaamalkovskaya.github.io/</a></li>
  <li>Demo screenshots: <a href="docs/screenshots/">docs/screenshots</a></li>
  <li>Demo video: in progress</li>
</ul>

<h2>What LuxPilot is</h2>

<p>
  LuxPilot is a governed AI advisory system for small business decision support.
</p>

<p>
  The goal is not to build a generic chatbot that gives fluent advice. The goal
  is to build an advisory system where strategic recommendations are:
</p>

<ul>
  <li>deterministic;</li>
  <li>inspectable;</li>
  <li>replayable;</li>
  <li>feasible for the user;</li>
  <li>bounded by confirmed inputs;</li>
  <li>explainable from traceable decision logic;</li>
  <li>safe for human validation.</li>
</ul>

<p>
  The LLM is useful, but bounded. It may extract candidate variables, explain an
  already-made decision, generate draft content, or ask adaptive questions. It
  does not choose the strategic route.
</p>

<h2>Why this matters</h2>

<p>
  Advisory and decision-support systems are tempting to build entirely on top of
  an LLM. But when the model sits on the decision path, several risks appear:
</p>

<ul>
  <li>the same input may produce different recommendations;</li>
  <li>it becomes difficult to replay why a decision was made;</li>
  <li>failures are hard to attribute to input, rules, prompt, or model;</li>
  <li>model or prompt changes can silently alter system behavior;</li>
  <li>fluent recommendations may ignore feasibility and user capacity.</li>
</ul>

<p>
  LuxPilot explores an alternative architecture: deterministic governance first,
  LLM execution second.
</p>

<h2>Core design principles</h2>

<h3>1. Deterministic routing</h3>

<p>
  Routing is performed by a deterministic engine over a confirmed profile
  snapshot and a versioned rulebase.
</p>

<p>
  The routing path does not call a model provider, use randomness, or rely on
  free-form generation.
</p>

<h3>2. LLM off the decision path</h3>

<p>The LLM does not choose the recommendation.</p>

<p>
  It can help with extraction, explanation, generation, and adaptive questioning,
  but only around a decision that is already structured and bounded.
</p>

<h3>3. Confirmed variables only</h3>

<p>
  LLM-extracted or inferred values are treated as candidates until confirmed.
</p>

<p>Unconfirmed variables cannot enter deterministic routing.</p>

<h3>4. Model-agnostic execution contracts</h3>

<p>LLM interactions are represented as function-like contracts.</p>

<p>
  This makes it possible to swap model providers without changing the
  deterministic decision layer.
</p>

<h3>5. Audit and replay</h3>

<p>
  A routing decision should be reproducible through a replay contract:
</p>

<pre><code>snapshot_hash + rulebase_version + engine_build -&gt; output_hash</code></pre>

<p>This supports later inspection of why a recommendation was produced.</p>

<h3>6. Evaluation built into the architecture</h3>

<p>
  The system is designed to support evaluation through synthetic fixtures,
  schemas, trace summaries, replay concepts, failure-mode analysis, and baseline
  comparison.
</p>

<h2>Demo screenshots</h2>

<p>
  The screenshot set shows the public LuxPilot demo flow using synthetic/demo
  data only.
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
  See the full screenshot set in
  <a href="docs/screenshots/">docs/screenshots</a>.
</p>

<h2>Repository map</h2>

<table>
  <thead>
    <tr>
      <th>Path</th>
      <th>Purpose</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="CASE_STUDY.md">CASE_STUDY.md</a></td>
      <td>Narrative case study: problem, governance boundary, walkthrough, and public artifact scope.</td>
    </tr>
    <tr>
      <td><a href="ARCHITECTURE.md">ARCHITECTURE.md</a></td>
      <td>System architecture and deterministic/probabilistic component boundaries.</td>
    </tr>
    <tr>
      <td><a href="PUBLIC_SPEC.md">PUBLIC_SPEC.md</a></td>
      <td>Sanitized public specification of the governed advisory system.</td>
    </tr>
    <tr>
      <td><a href="REPRODUCIBILITY.md">REPRODUCIBILITY.md</a></td>
      <td>Snapshot, audit, replay, and determinism design.</td>
    </tr>
    <tr>
      <td><a href="EVALS.md">EVALS.md</a></td>
      <td>Public evaluation plan and synthetic evaluation dimensions.</td>
    </tr>
    <tr>
      <td><a href="FAILURE_MODES.md">FAILURE_MODES.md</a></td>
      <td>Failure modes and mitigations for governed LLM advisory systems.</td>
    </tr>
    <tr>
      <td><a href="BASELINE_COMPARISON.md">BASELINE_COMPARISON.md</a></td>
      <td>Comparison against unguided LLM advice, generic checklists, beta flow, and governed architecture.</td>
    </tr>
    <tr>
      <td><a href="PUBLIC_ARTIFACT_BOUNDARY.md">PUBLIC_ARTIFACT_BOUNDARY.md</a></td>
      <td>What this public artifact demonstrates and what it intentionally excludes.</td>
    </tr>
    <tr>
      <td><a href="REDACTION_POLICY.md">REDACTION_POLICY.md</a></td>
      <td>Redaction and sanitization policy.</td>
    </tr>
    <tr>
      <td><a href="DISCLAIMER.md">DISCLAIMER.md</a></td>
      <td>Public case-study disclaimer.</td>
    </tr>
    <tr>
      <td><a href="SECURITY.md">SECURITY.md</a></td>
      <td>How to report suspected accidental exposure.</td>
    </tr>
    <tr>
      <td><a href="ROADMAP.md">ROADMAP.md</a></td>
      <td>Public artifact roadmap.</td>
    </tr>
    <tr>
      <td><a href="docs/">docs/</a></td>
      <td>Walkthroughs, diagrams, and screenshots.</td>
    </tr>
    <tr>
      <td><a href="docs/demo-walkthrough.md">docs/demo-walkthrough.md</a></td>
      <td>End-to-end public demo walkthrough.</td>
    </tr>
    <tr>
      <td><a href="docs/screenshots/">docs/screenshots</a></td>
      <td>Public demo screenshots using synthetic/demo data.</td>
    </tr>
    <tr>
      <td><a href="schemas/">schemas/</a></td>
      <td>Public JSON schemas for snapshots, routing results, contracts, and audit logs.</td>
    </tr>
    <tr>
      <td><a href="fixtures/">fixtures/</a></td>
      <td>Synthetic bakery and salon fixtures.</td>
    </tr>
    <tr>
      <td><a href="toy-rulebase/">toy-rulebase/</a></td>
      <td>Illustrative toy rulebase. Not production logic.</td>
    </tr>
    <tr>
      <td><a href="tests/">tests/</a></td>
      <td>Human-readable governance test specifications.</td>
    </tr>
    <tr>
      <td><a href="prompts/">prompts/</a></td>
      <td>Public prompt-hygiene examples. Not production prompts.</td>
    </tr>
    <tr>
      <td><a href="audit/">audit/</a></td>
      <td>Synthetic audit log and replay contract example.</td>
    </tr>
  </tbody>
</table>

<h2>What is public</h2>

<p>This repository may include:</p>

<ul>
  <li>high-level architecture;</li>
  <li>public system boundaries;</li>
  <li>synthetic fixtures;</li>
  <li>toy examples;</li>
  <li>public schemas;</li>
  <li>audit/replay examples;</li>
  <li>prompt-hygiene examples;</li>
  <li>human-readable governance tests;</li>
  <li>public screenshots;</li>
  <li>case-study documentation.</li>
</ul>

<h2>What is intentionally not public</h2>

<p>This repository does not include:</p>

<ul>
  <li>production codebase;</li>
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
  See
  <a href="PUBLIC_ARTIFACT_BOUNDARY.md">PUBLIC_ARTIFACT_BOUNDARY.md</a>,
  <a href="REDACTION_POLICY.md">REDACTION_POLICY.md</a>, and
  <a href="DISCLAIMER.md">DISCLAIMER.md</a>.
</p>

<h2>Status</h2>

<p>This is a documentation-first public case study.</p>

<p>The repository is intended to demonstrate:</p>

<ul>
  <li>governed LLM system architecture;</li>
  <li>deterministic/probabilistic boundary design;</li>
  <li>audit and replay thinking;</li>
  <li>evaluation readiness;</li>
  <li>responsible public disclosure without exposing commercial implementation details.</li>
</ul>

<h2>Intended audience</h2>

<p>This repository is intended for:</p>

<ul>
  <li>AI evaluation and model-behavior teams;</li>
  <li>AI safety and deployment teams;</li>
  <li>applied AI product teams;</li>
  <li>human-AI decision-support researchers;</li>
  <li>technical recruiters and hiring managers evaluating AI systems work.</li>
</ul>

<h2>Contact</h2>

<p>
  Larisa Somova / Malkovskaya<br>
  Portfolio: <a href="https://larisaamalkovskaya.github.io/">https://larisaamalkovskaya.github.io/</a><br>
  Live demo: <a href="https://luxpilot.app">https://luxpilot.app</a>
</p>

<h2>License and reuse</h2>

<p>
  This repository is provided as a public portfolio and research case-study
  artifact.
</p>

<p>
  No open-source license is granted for this repository. All rights are reserved
  unless explicit written permission is given by the copyright holder.
</p>

<p>
  You may view this repository for evaluation, hiring, academic review, and
  non-commercial reference purposes.
</p>

<p>
  The production LuxPilot system, rulebase, prompts, templates, and commercial
  implementation are not included and are not licensed through this repository.
</p>

<p>
  See <a href="LICENSE">LICENSE</a> for the repository license terms.
</p>
