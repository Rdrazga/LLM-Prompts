# AI Dungeon

AI Dungeon is an online text-based publishing platform to publish role-play (RPG) scenarios run by LLMs. The site allows for content of various ratings similar to ESRB ratings, and automatically moderates them using Claude.

## Disclaimer
I am not a part of latitude's team and this is not an official system prompt for Latitude or AI Dungeon, These are user-made system prompts that were created as a part of collaborating with Latitude's staff in their discord on making and testing the content moderation tool. AI Dungeon's actual content moderation System Prompt and Content Policy can be found on their website.

The content levels and rating instructions/descriptions match AI Dungeon's content policy at the time. They do not represent AI Dungeon's current moderation or content policy.

## How it works
The Moderation Instructions use an XML tag-based formatting to categorize instructions and process, and require the LLM to return an XML output. Due to Claude 3.7's training and tool use ability this works very well on Claude-based models or models with similar training on XML tags.

The system prompt induces an artificial form of reasoning within \<think> and \</think> tags before the output, As the model collects evidence on content within a given scenario to use in its rating. The model then outputs the numerical rating the scenario should receive within the \<rating> and \</rating> tags, delivers a verbal explanation to the user in \<explanation> and \</explanation>, and finally gives specific evidence on the reason for the rating.

The \<think> step is important for non-reasoning models and builds a specific process for the LLM to follow for rating a scenario. This gives it time to debate itself and properly pull evidence without hallucinating before giving a final rating. Outputting the final \<rating> first without reasoning leads to the LLM often hallucinating a rating quickly, then justifying with the explanation afterwards even if rated incorrectly often creating a very forced justification and evidence.

The \<rating>, \<explanation>, and \<evidence> would be displayed to the user so they know how their content was rated, why it was rated that way, and have an understanding of what they would need to change to achieve a lower rating. For Latitude's content moderation team this also allows them to quickly find specific points of contension within a scenario they need to review manually.

## Other Notes
### Testing
To test a piece of content for the system to moderate, put the scenario within story preview tags:

\<story-preview>

(Story goes here)

\</story-preview>

### XML vs Markdown instructions
Both XML and Markdown were tested for this instruction set. On GPT based models markdown performed better with XML giving inconsistent or non-sensical results. On Claude based models the XML format worked better with consistent outputs and good results.

Smaller models can be used less consistently with this instruction set, a reminder should be appended after the story preview on the exact output structure they should use.

### Safety and Abuseability
The instruction set was specifically made with potential bypasses in mind. High end models can often recognize most regular bypasses throwing a warning, lower end models are more likely to be bypassed with smart prompting within the scenario. For the specific use case manual review handles the rest of the edge cases that the moderation tool does not catch.

### Image ratings
This only applies for models with multi-modal image capabilities. The system can be fed an image along with the scenario (Often cover art connected to the scenario) to rate with the scenario. This section of the instructions can be seperated into a seperate call specific to the image or completely removed and replaced with a proper image moderation software.
