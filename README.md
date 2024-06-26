# Eval Scripts

This directory contains the scripts used to collect results for the framework described in our paper.

## Environment Setup

We used poety for dependency/environment management and fly.io to deploy and run our evaluation scripts. Only one machine is needed, so `fly scale count 1` was used immediately after `fly deploy -a APP_NAME`. We also used Azure for our GPT-4 inferencing, hence the setup in the script and all the extra env vars in the template. The data is pulled from supabase and written to CSV. Having data in CSV format will work with this codebase. We also use [LangSmith](https://smith.langchain.com/) for tracing/logging.  


## Description

The scripts load in the data and iterate through it, injecting it into the `eval.yaml` prompt and returning a result to be written to a table in [supabase](https://python.langchain.com/docs/integrations/vectorstores/supabase). The prompt gets the LLM to generate a categorical assessment of how well the prediction encapsulated the actual response (given the previous AI message as well).  

The `SUPABASE_MESSAGE_STORE` table is defined as follows:  

```sql
create table
  public.message_store (
    id serial,
    session_id text not null,
    user_id text not null,
    message_type text not null,
    timestamp timestamp with time zone not null default now(),
    message jsonb not null,
    constraint message_store_pkey primary key (id)
  ) tablespace pg_default;
```

The `SUPABASE_PAPER_RESULTS` table is defined as follows:

```sql
create table
  public.paper_eval (
    id bigint generated by default as identity,
    session_id text null,
    prediction_thought text null,
    actual text null,
    assessment text null,
    voe text null,
    conversation_turn smallint null,
    constraint paper_eval_pkey primary key (id)
  ) tablespace pg_default;
```

Any questions feel free to reach out to vince [at] plasticlabs [dot] ai.

## Reference

Use the following BibTeX citation to reference the paper

```
@misc{leer2023violation,
      title={Violation of Expectation via Metacognitive Prompting Reduces Theory of Mind Prediction Error in Large Language Models}, 
      author={Courtland Leer and Vincent Trost and Vineeth Voruganti},
      year={2023},
      eprint={2310.06983},
      archivePrefix={arXiv},
      primaryClass={cs.CL}
}
```
