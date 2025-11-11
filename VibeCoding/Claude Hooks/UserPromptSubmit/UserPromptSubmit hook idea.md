
I was testing GPT5 and found that it likes to articlate out loud it's thinking and what I needs to do. This got me thinking, since we need to manage context why not get Claude to do something similar, and this is the prompt addendum I have been using which has increased the accuracy and quality of Claude's output when coding.

There is also 'plan mode' but I find that isn't as effective and not everythign needs to be 'planned', rather that this prompt addendum does is ensure that Claude actually understands what I am asking and I can then clarify or correct anything which I dont think Claude understood correctly

Here it is, and I have been adding this at the end of all my user inputs:

"_Can you please re-articulate to me the concrete and specific requirements I have given you using your own words, include what those specific requirements are and for each requirement what actions you need to take, what steps you need to take to implement my requirements, and a short plain text description of how you are going to complete the task, include how you will use Sub-Agents and what will be done in series and what can be done in parallel. Also, re-organise the requirements into their logical & sequential order of implementation including any dependancies, and finally finish with a complete TODO list, then wait for my confirmation._"