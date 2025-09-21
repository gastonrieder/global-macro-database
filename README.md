# Global Macro Database Dashboard

**Live Dashboard URL:** [https://gastonrieder.github.io/global-macro-database](https://gastonrieder.github.io/global-macro-database)

---

Hi Dimi and team,

This is a small readme where I elaborate on my thought process. Normally this would not be necessary; there'd be shared context between us. The intention is to give you a better view into both my reasoning and communication skills without cluttering the dashboard.

I am really looking forward to hearing your feedback on the task. All the best.

## Data cleaning

I didn't reinvent the wheel here. I had a read of the documentation to understand the data and its limitations. Then I ran a few checks to make sure the data responded to the expectations outlined in the documentation. I also ran a couple data quality checks to make sure things made sense (ie., that there were no duplicate countries, ISO3 with less/more than 3 characters, etc).

## Problem-framing rather than EDA

Rather than explore data aimlessly, I set out to build a framework and solve a problem. I think this is worth highlighting because data requires a sense of direction; otherwise, we will just torture the data until it confesses to what we want.

Being explicit about our objectives and hypotheses helps us set the tone for the analysis, and steer it towards actionability. This means we stay intellectually honest and derive useful insights and lessons from the data.

## Rationale for the story

I wanted to write my analysis on growth and development (measured as GDP per capita). In Argentina, there is usually a conception of the world where it is almost impossible to bridge the gap with developed countries due to structural reasons rooted in colonialism. While this surely plays a role in the state of the world, surely it can't be all there is to it.

This led me to focus on countries that did develop post-WWII, namely Japan and South Korea. I built a narrative around the approach these countries took in contrast to two major Latin American countries I know about, Argentina and Brazil.

Just to conclude, I am no economist - I am sure my analysis could be scrutinised and criticised in countless ways. That said, I hope you find it interesting.

## Visualisation/storytelling

For the most part, I tried to limit the text and let the visuals tell the story. I like action-packed titles, callout boxes and reference points. These help users immediately focus on the main takeaway. I draw inspiration from the [Storytelling with Data book](https://www.storytellingwithdata.com/) and the [visual style guide](https://design-system.economist.com/documents/CHARTstyleguide_20170505.pdf) published by The Economist.

I pushed the SQL queries to the bottom, so users focus on the story first.
