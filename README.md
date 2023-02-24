# Portfolio
Portfolio for data analytics and visualizations

## Introduction

I have been a researcher in the life sciences for the past 22 years. I started my career in Portugal, where I grew up and did my undergrad studies. I then moved to Leiden, in the Netherlands to complete a PhD, where I was studying a genus of amphibians called *Triturus*. For this I learned a lot, from collecting tissue samples in the field, processing these samples in the lab, to analysing the resulting data. I have mostly focused on data analysis ever since, even though I occasionally return to the lab. My areas of research have changed from more basic research, such as my evolutionary biology work with *Triturus*, bees or marine mammals, to more applied, such as epidemiological work with Campylobacter, avian influenza or Mycobacterium, or psychiatric genetics working on schizophrenia, bipolar disorder or 22q11 syndrome.

I have about 30 publications in peer reviewed journals which have gathered close to 1000 citations (according to [Google Scholar](https://scholar.google.com/citations?user=KuLozM0AAAAJ) ), which I am proud of, but are not necessarily the highest ranking! I have been tracking when new publications cite my work, and it is clear that some of my papers gather more citations quicker than others. But I couldn't find a tool that showed me this!

So I took the opportunity that I am currently unemployed and my employment center offered a data analytics course run by Google and Coursera to test this hypothesis. This was supposed to be a business question but I couldn't help make it about me, haha! But the analysis here is generalizable, and I will show some examples of how this applies to other researchers and broad topics.

I will structure this according to the scheme that Google offers in the course, with the phases: *ask*, *prepare*, *process*, *analyze*, *share*, and *act*. I will pretend that this is a "legitimate" business case, althoug I genuinely believe that bibliometric analytics could be interested in these analysis.

## Ask
The case sudy packet suggest a number of questions to be answered in this phase:
1. What type of company does your client represent, and what are they asking you to accomplish?
This is a bibliometrics intelligence company, and they want to provide their clients with a tool to predict how many citations a research paper will generate based on the performance in the first year. For this they first would like to see some data analysis on a couple of test cases to see if this would be useful. Stakeholders are the company exacutives, and researchers that want to explore their citation network.
2. What are the key factors involved in the business task you are investigating?
Researchers do not have an easy way to see how much a paper is cited in relation to others, or other papers in the field, and how this "performance" changes throughout the course of time.

3. What type of data will be appropriate for your analysis?
I need citation data from a single researcher, or from publications within the same topic, or from a selection of papers that may be interesting to compare. I need a date for when citation was made. if day is not available, at least the year it was made. I also need the year the cited paper was published, so that I can calculate the difference between "year cited" and "year published". I will also calculate number of citations per year after publication and 

4. Where will you obtain that data?
Google Scholar citation data does not seem to be publicly available in a workable format. There is a growing collection of open citation data at [Open Citations](https://opencitations.net) which may be fairly complete, however due to time constraints I will use data from [Wikidata](https://www.wikidata.org), a project I am more familiar with. Citation data in Wikidata is most likely incomplete in many regards, so take any conclusions from there with a grain of salt. I may repeat this analysis with the Open Citation corpus, if I get a chance to become more familiar with it.
5. Who is your audience, and what materials will help you present to them effectively?
My "fictional" audience would be the executives of this bibliometric company, but more generally, this presentation is for anyone interested in bibliometrics, in particular researchers who may be keen on exploring citations to their scientific ouputs. This is not meant to game the citation landscape, and researchers would only publish papers that would get them more citations. There is enough of that already. This is meant as merely a curiosity driven exercise to get to know your citation landscaper better.

## Prepare

Thanks to the [Scholia tool](https://scholia.toolforge.org/) and the WikiCite Telegram group, I managed to quickly get the dataset I needed to analyse here. This is an adaptation of queries to Wikidata that Scholia uses for citation patterns across years, dividing by publication. [See this example](https://scholia.toolforge.org/author/Q21341624#citations-by-year). Wikidata is a free knowledge repository for structured data, and has a [CC-0](https://creativecommons.org/share-your-work/public-domain/cc0/) license, equivalent to Public Domain. As the data is crowdsourced, I am not certain of how complete it is. It is probably more complete for recent years, than it is for citations happening in the 1950's or 60's or earlier, as digitization of metadata is probably lagging for those decades.

I prepared four different queries, one for myself (h-index: 17, time of first publication 2007) (https://w.wiki/6N2E), one for long-lived academic [E. O. Wilson](https://en.wikipedia.org/wiki/E._O._Wilson) (https://w.wiki/6N5s) publishing since at least the 1950's until his death in 2021, one for the topic "Comparative Genomics" (https://w.wiki/6NVK), and the last one for the topic "Mycobacterium bovis" (https://w.wiki/6NVY).

Let's unpack one of those queries, to see what they do:

The first line chooses the default view to be a bar chart, to download the query results select "download"->"csv file"

`#defaultView:BarChart`

We then tell SPARQL to produce a table with columns `year` and `count` (a sum of citations of a work) and `workLabel` (the name of the work), and then a subquery (WITH) 

`SELECT ?year (count(distinct ?citing_work) as ?count) ?workLabel WITH {`

where we sum the number of distinct works citing our work of interest (matching the WHERE clauses)

`  SELECT (count(distinct ?citing_work) as ?totalCount) ?work WHERE {`

The work must have the same author (property P50 in Wikidata) as the one with the corresponding ORCiD (P465). If you change the ORCiD here you can get results for a different author.

`    ?work wdt:P50 / wdt:P496 "0000-0000-0000-0000" .`

This retrieves the identifier for a work that has the property `cites` matched to the work.

`    ?citing_work wdt:P2860 ?work .`

We then group our results by work

`  } group by ?work`

Order by descending order of the total count

`    order by DESC(?totalCount)`

Assign the result to the variable `PAPERS`

`} AS %PAPERS WHERE {`

Including the variable PAPERS in the index to improve performance

`  INCLUDE %PAPERS`

Again the name of the citing work

`  ?citing_work wdt:P2860 ?work .`

And the date

`  ?citing_work wdt:P577 ?date .`

Extract the year

`  BIND(str(YEAR(?date)) AS ?year)`

Use the wikidata label service to get readable names instead of just the Wikidata identifier

`  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en". }`

Group by year and name of work

`} group by ?year ?workLabel`

Excluding anything in 2021 or later

`  having (?year < "2021")`

Order by descending year

`  order by desc(?year)`


## Process
I downloaded the query results as .csv files and inspected them in Excel or Notepad++. In the beginning I had used VLOOKUP in excel to match the paper with the year of publication, based on a separate query, but I later figured out how to get it directly in that query, using SPARQL. It is better this way, because VLOOKUP is sensitive to how data is sorted and I was afraid to mess it up.
I am trying to use Tableau for data visualization, as it was covered on the course, and I didn't have experience with it. The interface is nice and is easy to use, but I am not sure if the data viz's will keep as I am not planning for now to subscribe to their plans (for some reason, it was not saving the viz on the Tableau Public, so I had to open a trial account). So, besides placing the visualization on a interactive dashboard, I tried saving some of the visualizations as images (see below). The dashboard for this exercise is on my profile at [Tableau](https://dub01.online.tableau.com/#/site/themudo/views/citations/Painel1?:iid=1). I also tried some Tableau alternative, such as MS Power BI, Talend Open Studio, Plotly-Dash, etc. But except for Power BI, it was not easy to quickly replicate the data visualization of Tableau.

I removed papers without year of publication from the analysis. If needed I could have checked if that information was available elsewhere, and either added it directly in Wikidata (I might do that later anyway), or just to my csv file. I chose not to do so, because I had plenty of other data and was eager to proceed to the visualization. There were only a couple of papers from E. O. Wilson that didn't have that information on Wikidata.

## Analyze
Citation data was aggregated by year of citation, and work (i.e. paper). `Years since publication` was calculated by substracting the year the work was published to the year the citing work was published (`Year_since_publication = citing_work_year - cited_work_year`). This variable is an integer >= 0. This was calculated in order to be easier to compare performance between papers.

E. O. Wilson's data had some interesting characteristics. For example, citations were sparse from most of the 1960's through the 2000's. There could be several explanations for this, I think. First, the internet wasn't really around for much of that time, so citations from that period may not be in digital form and are therefore not available; Wikidata may also be biased to more recent publications; So, there are more citations for those periods, but they are not included in the dataset. Another explanation is that the number of academic publications has also been increasing exponentially (see for example [this publication from the NSF](https://ncses.nsf.gov/pubs/nsb20214/publication-output-by-country-region-or-economy-and-scientific-field), allowing each paper to be cited more often in more recent years.

Some publications did not receive citations immediately after publication, but took a number of years before getting them. This could be due again to lack of coverage for earlier years. This hypothesis seems to be corroborated by the fact that removing very early publications also removed most of the publications with this pattern.

From some of the datasets it seems that papers are mostly cited in the first ten years after publication. Citations usually lag one year after publication, but are clearly indicative already of the relative influence of the paper.


