# Portfolio
Portfolio for data analytics and visualizations

## Introduction

I have been a researcher in the life sciences for the past 22 years. I started my career in Portugal, where I grew up and did my undergrad studies. I then moved to Leiden, in the Netherlands to complete a PhD, where I was studying a genus of amphibians called *Triturus*. For this I learned a lot, from collecting tissue samples in the field, processing these samples in the lab, to analysing the resulting data. I have mostly focused on data analysis ever since, even though I occasionally return to the lab. My areas of research have changed from more basic research, such as my evolutionary biology work with *Triturus*, bees or marine mammals, to more applied, such as epidemiological work with Campylobacter, avian influenza or Mycobacterium, or psychiatric genetics working on schizophrenia, bipolar disorder or 22q11 syndrome.

I have about 30 publications in peer reviewed journals which have gathered close to 1000 citations (according to [Google Scholar](https://scholar.google.com/citations?user=KuLozM0AAAAJ) ), which I am proud of, but are not necessarily the highest ranking! I have been tracking when new publications cite my work, and it is clear that some of my papers gather more citations quicker than others. But I couldn't find a tool that showed me this!

So I took the opportunity that I am currently unemployed and my employment center offered a data analytics course run by Google and Coursera to test this hypothesis. This was supposed to be a business question but I couldn't help make it about me, haha! But the analysis here is generalizable, and I will show some examples of how this applies to other researchers and broad topics.

I will structure this according to the scheme that Google offers in the course, with the phases: *ask*, *prepare*, *process*, *analyze*, *share*, and *act*. I will pretend that this is a "legitimate" business case, althoug I genuinely believe that bibliometric analytics could be interested in these analysis.

##Ask
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

### A clear statement of the business task you have selected to investigate 
### A description of all data sources used 
### Documentation of any cleaning or manipulation of data 
### A summary of your analysis 
### Supporting visualizations and key findings 
### Based on what you discover, a list of additional deliverables you think would be helpful to include for further exploration 
### Your top high-level insights based on your analysis
