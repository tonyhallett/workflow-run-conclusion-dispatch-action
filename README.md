# About

This action is to be used with the workflow_run event.  It allows for a finer grained response to a workflow run.
A workflow run that reacts to a workflow run can only filter by activity types completed and requested and not by the conclusion state.
This action will create a repository_dispatch event with an event type that includes the conclusion and has the original payload.  By using this action there is no need for conditionals in a workflow run.
Set eventNameInEventType to true to include the event e.g pull_request in the event name.

The event type is ${workflowName} - ${event ( optional)} - ${conclusion} and client_payload is the workflow run payload.

# Example
```
name: Repository dispatch of AWorkflow
on:
  workflow_run:
    workflows: [AWorkflow]
    types: [completed]

jobs:
  workflow-dispatch:
    name: repo dispatch event with conclusion 
    runs-on: windows-2019
    steps:
      - name: repo dispatch
        uses: tonyhallett/workflow-run-conclusion-dispatch-action@v1.0.0
        with:
            GITHUB_PAT: ${{ secrets.PAT }}
```
and handling when successful (the repository dispatch)
```
name: AWorkflow - success
on:
  repository_dispatch:
    types: [AWorkflow - success]

jobs:
  do-something-with-payload:
    # create a step that uses github.event.client_payload
    
```