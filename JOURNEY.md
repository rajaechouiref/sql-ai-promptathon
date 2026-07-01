# Journey: From star ratings to hidden frustration

## The goal
We started with a simple question: can a product review be misleading when the rating looks good but the text tells a different story? We wanted to test whether Capterra ratings were hiding real frustration behind otherwise positive-looking scores.

We also wanted the work to feel practical. Not just an analysis report, but something that could be run again, inspected, and used on real reviews.

## Turn by turn
1. We loaded the Capterra review dataset and inspected the structure.
2. We moved the data into a SQL-friendly format so the analysis could be queried and reused more cleanly.
3. We scored the review text with sentiment analysis and compared it to the star rating.
4. We flagged reviews where the stars looked positive but the text did not, which became our Liar Score.
5. We grouped those reviews by tool and looked for the patterns behind the hidden frustration.
6. We added complaint aspect classification so the hidden problems became more concrete: support quality, pricing, features, ease of use, performance, and integration.
7. We added semantic search so similar complaints could be found across tools even when the wording was different.
8. We built a Red Flag Detector so a single review could be checked quickly and labeled as likely hiding something or looking genuine.
9. We built a recommendation engine that ranked tools by the complaint patterns that mattered most for a given concern.

## What we found
The analysis showed that some reviews with strong ratings still contained clear frustration in the text. The pattern was not random. It repeated across tools and often pointed to specific complaint areas rather than general negativity.

We found that the Liar Score varied by tool. Some products had a much higher share of reviews where the stars looked better than the writing. We also found that the hidden complaints were usually concentrated in a few recurring themes, especially support quality, features/functionality, and ease of use.

## What failed
We tried a heavier transformer-based approach first, including a BERT-style path for sentiment and similarity. It looked promising on paper, but it crashed under the memory limits of the environment. That forced us to pivot to something lighter and more reliable.

The simpler alternative was VADER. It was not as glamorous, but it was fast, stable, and good enough to make the analysis useful.

## What we almost got wrong
We also explored a sub-rating heuristic that tried to infer hidden frustration from separate review fields such as ease of use, customer service, or value. Early on, it looked like it might be a shortcut to the same answer. But when we checked it manually, it turned out to be too aggressive and produced a lot of false positives.

The first version claimed that 82.7% of the flagged cases were false positives. After manual validation, we rejected that approach. Only about 15% of the cases looked like real errors once we looked at the review text directly.

That was an important lesson: a shortcut can look efficient, but it is not trustworthy if it does not match the actual human language in the review.

## Final results
The final notebook now includes:
- a Liar Score per tool,
- the top complaint aspects behind hidden frustration,
- a Red Flag Detector for individual review text,
- and a recommendation engine that ranks tools by the complaint categories that matter most.

The result is less like a dry technical report and more like a practical review-reading tool.

## Tools used
- SQL MCP for database-backed analysis and inspection
- VADER for lightweight sentiment scoring
- sentence-transformers for semantic search across complaint text
- SQLite for a simple, rerunnable local data store

## Honest takeaway
This was not a perfect system. It was a useful one. The notebook makes the hidden mismatch between ratings and text much easier to see, and it gives a more honest picture of how customers really feel than star ratings alone.
