# Validation

## Introduction

Whenever data crosses a trust boundary, validation of some form must be done in order to help ensure the data is trustworthy at even a basic level. Whether the data are user credentials or data from a trusted client of it should be validated. Of course, in order to know when to validate data requires an understanding of your applications trust boundaries. There are several ways to determine trust boundaries; some are listed in the Determining Trust Boundaries section below. Once these boundaries are determined the next step is to identify the flow of data and when data must be validated, this is detailed in the section below titled: When to Validate. Finally, the actual act of validation is described in the How to Validate section. After these sections, this chapter will provide several examples of validation and validation failures to look out for. At the end of the chapter an exploration of finding vulnerabilities related to validation will be provided.

Before proceeding, please understand a very basic belief that I hold dear, which is to not let process prohibit you from getting things done. The following sections are an exploration on ways to help determine when and how to validate data as well as how to ensure you validated data correctly. My goal is not to dictate a formal process that you must adhere to. Instead, these sections are meant to push the way you approach validation and application development.

### Determining Trust Boundaries

There are several varying ways to determine trust boundaries.  The company you work for may have guidelines in place for you on doing just this.  For example, if the company you work for follows the Software Development Lifecycle (SDL) then they may recommend that you create data flow diagrams (DFD).  If you are familiar with data flow diagrams and prefer to take the SDL route then look at the SDL Threat Modeling Tool that will use a DFD created with Visio to help identify areas where validation is critical.

Perhaps the largest key with any determination of trust boundaries is to take a step back from the application and take a high level view of how data will flow through it and where the data is coming from.  If the data is coming from an external source then there is a need to ask yourself and the developers of the application if the data can be trusted.  Take a defense in depth approach to validation.  In other words, don’t give data coming from a database a pass simply because the database is only sent data from your application.  This doesn’t mean to be paranoid about everything; it means to think about the application from a broader perspective and to engage other developers in a dialogue about the trustworthiness of data sources.

In the case of a database that only your application uses, are business analysts able to access the data and modify it?  The data going into the database may be completely clean and trusted, what about the data coming out of it?  Can a disgruntled employee tamper with data in the database and cause damage your application?

Determining if data coming from a potentially untrustworthy data source depends on the application and the type of data the application interacts with.  For example, if the application is a simple internal employee lookup and only stores the employees name and department, it will likely be overkill to validate data coming back from the database storing this information.  However, if for example, the data source is used by 3rd party contractors then it will likely be useful to validate the data coming in from this data source.

### Diagramming Trust Boundaries

Creating a DFD is a great way to visualize how data sources interact with the application you are creating.  This can be done with a variety of tools from Microsoft Visio and creately.com to using a piece of paper and a pen.  Theses diagrams can often become quite complex, therefore I always find it best to start simply.  Draw a square and write the words “My App” in it.  From here, draw other shapes that represent anything your application will interact with.  After this it’s simply a matter of drawing arrows pointing in the direction that data flows to and from your application.  All of the trust boundaries are likely now determined for you, simply draw a red circle around the square representing your application.  These are the trust boundaries, now go through each of the arrows that points in toward your application and decide if the data can be trusted completely.  One helpful tip, especially if you are under a time crunch, is to rank the trust boundaries in order of trustworthiness.  For example, the user of your application whom you have never met is likely to be less trustworthy than the database where you are storing application data.  Below in Figure 2.1 is an example of a DFD with trust boundaries identified and ranked.  Keep in mind that the application can be broken further into future diagrams and that the first step is to create a visualization of where the data is and isn’t trusted.

<img src="figures/2.1.png" />

***Figure 2.1***

### Discussing Trust Boundaries

Likely the most successful way to determine an applications trust boundaries is to talk to others on your team about them.  However, my recommendation is that the meeting be small, with only one or two people.  This meeting must be focused and shouldn’t be an exhaustive list of ways that your application or new feature can be hacked, but rather the point is to determine where data is coming from and if it should be trusted.  Therefore, before the meeting takes place send the other participants a DFD with where you think the trust boundaries exist.  Give the members time to review the diagram and then schedule a meeting.  The itinerary below in Figure 2.2 is one that I find useful for these types of meetings.

* What trust boundaries are missing from the diagram
* What data is missing from the diagram
* Should the data that is trusted really be trusted

After the meeting is finished update the diagram and have it validated by members of the team before proceeding further.  It’s entirely likely that after development begins on a feature or the application that you will decide a data source can or cannot be trusted.  When this happens you can update your diagram if you would like.  This is a decision that really depends on the review process of the application.


### Validation Assumptions

Hopefully, a party outside of your immediate development team will review the application before it is released.  If this is the case then you should update the diagram as you can offer it to the other parties involved to validate your assumptions and ensure that the data coming from untrusted sources is validated appropriately.

If the party you are handing the diagram doesn’t understand the diagram, use this as an opportunity to educate them.  Please do not use it as an excuse to cut corners with protecting the application.  Educate QA or another team about how to try to compromise the application.  The more knowledgeable those involved in the review portion of the application are the more secure your application is likely to become.

Hopefully, the third party that is reviewing your code is the open source world.  If this is the case, when creating new features or a new Node.js module provide the diagrams you created.  If you lack any diagrams then simply describe in an issue or wiki where you assume trust boundaries exist and where data needs to be validated.

### Reasons to validate
