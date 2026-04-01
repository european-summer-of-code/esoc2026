In order to be eligible for a stipend through the `sktime` agentic track, you must
have worked on an agentic or genAI related feature or application in the vicinity of
`sktime`, under the GitHub account that you are applying with.

Some ideas for this are listed below:


### sktime-mcp

[sktime-mcp](https://github.com/sktime/sktime-mcp) is a recent prototype for
`sktime` MCP agentic integration.

Valid contributions to this projects are:

* attempt to set up the MCP with your agent. Report failures, lack of documentation,
  or lack of clarity of documentation, in the `sktime-mcp` issue tracker.
* ask coding tools using the MCP to replicate common workflows in the `sktime` tutorials,
  or use cases of `sktime` that you find interesting.
  Where not successful, suggest designs for MCP tools to enable those workflows or use cases.
* contribute directly to the `sktime-mcp` code base: documentation, good first issues, tools, CI, etc.


### LLM-based forecaster

Recent approaches use LLM agents to construct a forecaster, or, more generally,
an estimator or pipeline, based on data and an English language prompt, possibly
also returning an English language prompt.

Implementing such a forecaster or algorithm would be an exciting addition,
also see https://github.com/sktime/sktime/issues/9721

Since this idea does not have a unique specification, multiple people can work on this
and propose how to do this.


### Foundation models

Integrate a foundation model into `sktime`: https://github.com/sktime/sktime/issues/6177

Not explicitly on the list but also very interesting are foundation models - or other approaches -
that are able to make use of textual data or added data related prompts.


### your own idea!

Your own idea for a genAI or agentic feature around `sktime` is appreciated - go wild!

Please ensure to provide code in a permissive license or copyleft repository,
and a reasonable amount of documentation for your project.

If you require tokens for testing your idea beyond what free tiers offer,
please get in touch on the `sktime` discord `dev-chat` to request tokens for your idea.
