# Design Document: AI Proposal Triage (MATACS-TH-2026-02)

Proposals will be labelled as A-E. Steps below relate to diagram.

1. AI v Rules

AI (steps 4 and 7):
Step 4: Pull data from the text of proposal (Licence, Price, Weeks, Warranty Years, Install Numbers, Team). All must have a short quote to back them up.
Step 7: Score 'Team and Approach'. This is a fixed rubric, scoring based on named leader, crew, safety plan and monitoring plan. The only subjective criteria.

Rules (All other steps):

Extract text from the PDF, look for hidden instructions, ensure there are quotes, pass/fail criteria, price, timeline, warranty and experience scores. Weight scores and sort.

This line is here due to the AI good at understanding unstructured text. The rules are better at making sure criteria is fixed and accurate. As only the AI is extracting data and the rules are scoring, there is no way the AI can influence the order of proposals by being creative.

2. Consistent & Explainable

Criteria and weights are taken from the admin database, not the prompt. There are no hard coded scores in the prompt. AI steps are run with temperature 0 and have a fixed JSON output. Output cached using (file hash, prompt version, model version). Scored using fixed formulas in the code. Same data will score the same. The same set of proposals will always produce the same result as we have cached data. Any differences would be small changes in the AI text. However, we have a check to ensure there are quotes in the text and the output is fixed to a schema. Any changes to the data will be highlighted. The reviewer can see the breakdown of scores per criteria, along with what the data was extracted as and the quote from the proposal. A single line reason will be generated for each ranking.

3. Human Intervention

The reviewer selects the category they want and uploads the files. Proposals with issues will be highlighted to the reviewer. They can ask the bidder for clarification or accept the proposal with a note. The reviewer will see the ranked proposals. They can change the extracted data and override the ranking with a reason. The final award will be done by a person. Nothing is awarded by the tool. Audit Trail: Who uploaded what, file hash, criteria version, prompt version, model version, raw AI outputs, all extracted data with quotes, all scores, all issues, all overrides with reason and who did it, final decision and time stamp.

4. Changing Criteria

Criteria are versioned. All scores will have the version of criteria used. If admin changes weights then previous results will show as out of date with a warning. As the data is extracted we can re-score the proposals with new weights. This will not incur any new costs as we are just running the rules again. If the mandatory criteria or new criteria has been changed then the relevant data extraction would need to be run again.
The reviewer can see how the proposals were ranked previously. Audit trail holds this data.

5. Risks & Mitigations

Risk Mitigation:

Incorrect Data extracted All data must have a quote. Code ensures there is a quote in the text. If there isn’t then it will be highlighted. The reviewer can see and fix the data.

Falsified Data If there is no data then it will return as null. Nothing will be guessed. If there is no data for mandatory or scoring criteria then it will be highlighted.

Manipulate proposals The text of proposals will be treated as data. We will search for patterns and highlight any suspicious phrases. The AI will not influence the weights or ranking. Any issues will need to be checked by a human (proposal C in sample set).

Unclear statements Mandatory criteria must be stated by the bidder. If they say they will employ someone in the future then it won’t pass (proposal B in sample set).

Bias Longer or well written proposals will be favoured. Only 15% of the score is using the AI. The rubric is fixed with yes/no criteria. No company names are used in scoring. The result will be checked by humans.
Poor PDF quality (scans, tables) Poor quality data extraction will be highlighted.

Cost Only 2 steps use the AI. Data is cached so nothing will be lost when re-scoring.

Over reliance The output will be shown as a suggestion. The decision will be made by a human. Overrides are logged.