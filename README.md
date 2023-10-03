# Eval Scripts

This directory contains the scripts used to collect results for the framework described in our paper.

## Environment Setup

We used poety for dependency/environment management and fly.io to deploy and run our evaluation scripts. Only one machine is needed, so `fly scale count 1` was used immediately after `fly deploy -a APP_NAME`. We also used Azure for our GPT-4 inferencing, hence the setup in the script and all the extra env vars in the template. The data is pulled from supabase and written to CSV. Having data in CSV format will work with this codebase.

## Description

The scripts load in the data and iterate through it, injecting it into the `eval.yaml` prompt and returning a result to be written to a table in supabase. The prompt gets the LLM to generate a categorical assessment of how well the prediction encapsulated the actual response (given the previous AI message as well).  

Any questions feel free to reach out to vince [at] plasticlabs [dot] ai.


TODO: Describe the table schema and the env template
