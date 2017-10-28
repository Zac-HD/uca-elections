# uca-elections

*Electoral tools for the Uniting Church in Australia*

## How the election works

Elections in the Uniting Church can be significantly more complicated than
most political elections.  Let's take the Assembly Standing Committee as our
example (and my motivation here):

- The committee consists of 6 ex-officio members, and 18 elected from the
  membership of the Assembly (~300 people).  (the committee may later co-opt
  up to four additional members, but that's irrelevant to the election)
- There are a variety of quotas\*:
  - The number of ministers may not be greater than that of lay people
  - Of the elected members, there must be at least one and not more than
    five from each Synod (state-level council)
  - At least two members under 25 years old
  - At least two members from CALD communities (ie non-English-speaking)
  - At least ten women and at least ten men

Suffice to say that even with only a few hundred votes in a single electorate,
counting this by hand is hard work!

In the current system, each elector approves of up to 18 nominees (of usually
30-40).  The returning officers then construct an ordered list of nominees,
and fill quotas preferring higher-ranked nominees.  However, the results may
vary depending on the order in which quotas are considered.

Instead, electors could approve of any number of candidates, and a computer
program outputs the committee membership which has the highest total approval.

In case you hadn't guessed, this is that computer program.

\* *Some of this is required by part 3.7.5.1 of
[the regulations](https://assembly.uca.org.au/resources/regulations),
the rest is convention*


## About the program

*Note: currently a draft, more exploring ideas than finished product.*

### Elector

`elector.py` takes a CSV file with a row for each nominee (name, votes,
categories), and outputs the best committee it can find.

- First, it uses the traditional approach to find a 'baseline candidate
  solution'
- Then tries a fairly large number of random combinations, looking for one
  with a higher score
- Then attempts an exhaustive search for the optimal solution

The result is the best combination that this process could find.

The implementation does a number of fancy things to reduce the search space
without affecting the result - the naive approach would have to check
between 86 million (30 nominees) and 18 *trillion* (50 nominees) combinations.
The main optimisations are to handle nominees who are certainly elected or
certainly not elected before starting (reducing both terms of 'n choose k'),
and to avoid generating and checking combinations which are invalid under
the quota rules or have a lower score than the best combination so far.

Initial speed results are ~10^5 combinations per second per core, which means
this can work *if* the program is reasonably smart - the naive approach takes
a few minutes for 30 nominees, but several months for 50!


### Counter

It would be cool to use OpenCV or something to read photos of the ballot
papers, to speed up data entry.  Maybe once the basics are working!
