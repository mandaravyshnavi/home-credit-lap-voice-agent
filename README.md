# Home Credit Loan Against Property (LAP) Voice Agent
## Technology

-   **LLM:** OpenAI `gpt-4o-mini`
-   **Framework:** Bolna
-   **Secrets:** Environment variables; never commit API keys

> This project is configured around OpenAI for language-model responses.
> The exact speech-to-text and text-to-speech providers depend on the
> pipeline you configure in Bolna. Do not describe the entire audio
> pipeline as OpenAI-only unless both audio input and audio output are
> configured and tested with OpenAI-compatible providers.

## What the agent does

The assistant can: - Explain the LAP qualification process in plain
language. - Ask for the applicant's property location and property
type. - Ask whether the property is residential or commercial. - Ask
about ownership and available property documents. - Ask for an estimated
property value and requested loan amount. - Ask about income and
supporting documents. - Explain when a case may need human review.

The agent provides preliminary guidance only. It does not approve or
guarantee a loan, quote a binding offer, or replace Home Credit's
official eligibility checks.

## Conversation guidelines

The assistant should: 1. Introduce itself as a Home Credit LAP
information assistant. 2. Ask one question at a time and summarize the
information provided. 3. Avoid requesting account passwords, OTPs, full
payment-card details, or unnecessary sensitive information. 4. Never
invent interest rates, fees, loan limits, turnaround times, or
eligibility rules. If verified information is unavailable, say so and
direct the customer to an authorized Home Credit representative. 5.
Treat agricultural land, unclear ownership, missing documents, and
out-of-service-area properties as cases requiring confirmation against
official policy---not as final automated decisions unless approved rules
are supplied. 6. Clearly tell the customer when a human representative
is needed.

## Requirements

-   Python version supported by the installed Bolna project
-   An OpenAI API key
-   Any additional credentials required by the selected audio pipeline

## Setup

### 1. Create and activate a virtual environment

Windows PowerShell:

``` powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 2. Install the project

From the repository root:

``` powershell
pip install -e .
```

If you are using the repository's documented dependency installation
instead, follow its current setup instructions.

### 3. Configure environment variables

Create a local `.env` file based on `.env.sample`, or set the variables
in your shell. Add your OpenAI key:

``` env
OPENAI_API_KEY=your_openai_api_key
```

Do not put real API keys in source code, screenshots, README files, or
Git commits. Keep `.env` out of version control.

### 4. Configure the OpenAI model

For a Bolna `LlmAgent`, the OpenAI configuration used by the example is:

``` python
llm_agent = LlmAgent(
    agent_type="simple_llm_agent",
    agent_flow_type="streaming",
    llm_config=SimpleLlmAgent(
        provider="openai",
        model="gpt-4o-mini",
        temperature=0.3,
    ),
)
```

Use the model/configuration types supported by the version of Bolna
installed in this repository. Do not mix this OpenAI configuration with
Gemini provider settings.

### 5. Run the agent

For a text-only test, use the repository's
`examples/text_only_assistant.py` after configuring it with the Home
Credit LAP system prompt and OpenAI model:

``` powershell
python .\examples\text_only_assistant.py
```

For a live voice call, configure and test the transcription, synthesis,
and telephony components required by your Bolna version. A working
text-only OpenAI LLM configuration does not by itself prove that the
full voice pipeline is configured.

## Suggested Home Credit LAP system prompt

Use this as a starting point for the agent's system instructions:

``` text
You are a polite and professional Home Credit Loan Against Property (LAP)
information assistant.

Your job is to explain the general application process and collect preliminary
details one question at a time:
1. Property location (city and area)
2. Property type
3. Whether it is residential or commercial
4. Whether the applicant owns the property and has supporting documents
5. Approximate property value
6. Requested loan amount
7. General income/source-of-income information and availability of supporting
   documents

Be clear that this is preliminary information collection, not loan approval.
Do not invent Home Credit policies, serviceable locations, interest rates, fees,
loan limits, or eligibility criteria. If a rule has not been provided in
verified company materials, say that a Home Credit representative must confirm
it.

If the property is agricultural, ownership is unclear, documents are missing,
or the location may be unsupported, explain that the case needs confirmation
by an authorized representative. Do not claim to make a final eligibility
decision unless verified rules explicitly authorize it.

Ask only one question at a time. Summarize the customer's answers and confirm
important details. Never ask for passwords, OTPs, or full payment-card details.
Be respectful, concise, and transparent.
```

## Security

-   Keep `.env` local and add it to `.gitignore`.
-   Never commit API keys, tokens, phone numbers, customer records, or
    call recordings.
-   Use test data while developing.
-   Follow applicable privacy and consent requirements before recording
    or processing calls.

## Project structure

This project builds on Bolna. Preserve the framework's core modules,
dependency files, examples needed for the demo, and the upstream
`LICENSE`. Add or modify only the files needed for the Home Credit LAP
agent.

