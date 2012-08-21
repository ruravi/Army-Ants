RFP-App-Matcher
=================
Team Army Ants
===================================
Contributors: 	RaviSundaram Rukmani,  
  		Sharan Munyal,  
		Thirumurthi Pragadheeshwaran

Scraperwiki Script	: Python located at \<TBD\>  
Framework		: Spring MVC


Problem Statement
==================
By 2014, the Federal government will spend an anticipated $112 billion per year on IT.  
State and local governments will spend approximately $60 billion. These expenditures result in a steady stream of
government RFPs, which include detailed project requirements and specs. For some of these RFPs, there might be an existing open source solution that could meet the project requirements and save the government from building custom software from scratch. Doing so could save cash-strapped local governments valuable resources that are desperately needed elsewhere. But a lack of information and burdensome procurement processes mean that government IT purchasers are rarely aware of available open source options. The Code for America Commons, a searchable directory of civic technology, attempts to address this problem by cataloging and promoting open source software available for government use. We see an opportunity to take this a step further and deliver timely, relevant information directly to the government officials who are posting these software RFPs. Enter RFP Alerts: a stand-alone app that will alert procurement officers when their RFP matches existing open source software listed in the Code for America Commons.

Our Solution
=============
This application allows users aka government officials to upload RFP files to their account.   
We search the Civic Commons repository for applications mathcing the user's RFP  
The user gets a list of mathcing software for the uploaded RFP

Implementation Details
=======================

Scraper Wiki  
> This python script mines the civic commons web site once a day and populates a database with all the applications in the civic commons repository  
  The values obtained from the scraper wiki script (JSON objects are parsed) are periodically added to our data store

MongoDB Interface  

> Two mongoDB data collections are maintained  
  1. CIVIC_COMMONS_COLLECTION  : contains AppType <ID, AppName, AppDescription, AppURL>  
     Holds the list of all Software applications in Civic commons repository along with its Description and URL  
  2. RFP_COLLECTION            : contains RFPCollectionType <ID, UserName, RFPName, RFPBody>  
     Contains an RFP title, body and the user associated with it, a list of app matches populated as a result of calling search in Lucene Indexer
	

Apache Lucene

> LuceneIndexer performs intelligent search with the description of the RFP as the query to search our Civic Commons repository. The LuceneIndexer search method runs periodically to perform indexing and adds all the documents to its in-memory RAMIndexer
		
User Interface

> We authenticate users with their twitter account  
The list of RFPs uploaded by the user are listed in the dashboard page  
The uploaded RFP is added the the RFP_COLLECTION store and used as query to fetch matching apps from the CIVIC_COMMONS repository  
Clicking on SearchNow will return the top 10 matches for each RFP	