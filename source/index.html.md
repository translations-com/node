---
title: GlobalLink Connect API Reference

language_tabs:
  - javascript: NodeJS

toc_footers:
  - <a href='http://www.translations.com/globallink/demo.html'>Free consultation</a>
  - <a href='http://www.translations.com/globallink/contact.html'>Contact Us!</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:


search: true
---



# GLExchange - Main Class

```javascript
class GLExchange {
	getProject(projectShortCode) {}
	initSubmission(submissionObject) {}
	uploadTranslatable(filePathLocal) {}
	startSubmission() {}
	getCompletedTargetsByProject(projectTicket, maxResults) {}
	getCompletedTargetsBySubmission(submissionTicket, maxResults) {}
	getCompletedTargetsByDocuments(documentTicket, maxResults){}
	downloadCompletedTarget(targetTicket, downloadDir) {}
	sendDownloadConfirmation(targetTicket) {}
}
```

This is the main API class. It will initialize a connection to Project Director. Different methods are available to perform the necessary operations: 

Method | Returns | Sumary 
------ | ------- | -------
`getCompletedTargetsByDocuments` | Array | Get completed targets for a given source document ticket
`getCompletedTargetsByProject` | Array | Get all completed targets for a given project shortcode
`getCompletedTargetsBySubmission` | Arrat | Get all completed targets for a given submission ticket
`getProject` | Array | Get project configuration by it`s shortcode
`startSubmission` | String | Start Submission

Method | Parmeter | Sumary 
------ | ------- | -------
`downloadCompletedTarget` | `maxResults` | Download all completed targets and returns `targetTicket`
`sendDownloadConfirmation` | `targetTicket` | Sends confirmation that the target resources was downloaded successfully by the customer
`initSubmission` | `submission` | Initialize a new Submission where `submission` object contains the PD submission configuration
`uploadTranslatable` | `document` | Uploads the document to project director for translation and returns `documentTicket`

# Models Classes

## Project

```javascript
class PDProject {
	constructor() {
		this.shortcode = '';
		this.name = '';
		this.ticket = '';
		this.languageDirections = [];
		this.fileFormats = [];
		this.workflows = [];
		this.customAttributes = [];
	}
	initProjectData(externalProject) {
		this.name
		this.shortcode
		this.ticket
	}
}
```

A project is a collection of configurations on the PD server, which includes language pairs, translation memory, workflow configuration, file-format parsers and user roles that govern the translation workflow.
While integrating with content systems, you can have multiple projects setup on project director and have the business user choose which project they want to submit the content to.

Method | Returns | Sumary 
------ | ------- | -------
`this.shortcode` | String | The project for which this submission will be created
`this.name` | String | The name of the submission that will show up in Project Director
`this.ticket` | String | The submission ticket
`this.languageDirections` | Array | Languages into which we will be translating the submission
`this.workflow` | Array | The workflow to be used
`this.customAttributes` | Array | The custom list of configured attributes for the project

## Submission

```javascript
class PDSubmission {
	constructor() {
		this.customAttributes = null;
		this.dueDate = 0;
		this.instructions = '';
		this.isUrgent = false;
		this.sourceLanguage = '';
		this.targetLanguages = [];
		this.metadata = null; // [{key1: value1}, {key2 : value2},...]
		this.name = '';
		this.projectTicket = null;
		this.pmNotes = '';
		this.submitter = null;
		this.ticket = null;
		this.workflow = null;
	}
}
```

A submission is a collection of Documents submitted to project director for a specific project. Within a submission a user can request for translation of a Document into one or more languages.

Method | Parmeter | Sumary 
------ | ------- | -------
`this.customAttributes` | `customAttributes` | [Optional] Custom attributes. Array of PDCustomAttribute. PDProject.customAttributes gets you the list of configured attributes for the project.
`this.dueDate` | `dueDate` | [Optional] Due date by when the submission should be completed in Project Director. This should always be greater than current date. If due date is not specified, Project Director will rely on the configured language model to calculate a due `date. strtotime*1000`
`this.instructions` | `instructions` | [Optional] Instructions for the translators
`this.isUrgent` | `isUrgent` | [Optional] Submission priority. Boolean. Defaults to false(Normal)
`this.sourceLanguage` | `sourceLanguage` | Source Language of the document in xx-XX format.
`this.targetLanguages` | `targetLanguages` | Target languages into which this document will be translated
`this.metadata` | `metadata` | [Optional] Key-value array of metadata
`this.name` | `name` | Name of the submission that will show up in Project Director
`this.projectTicket` | `projectShortcode` | Set the PDProject for which this submission will be created
`this.pmNotes` | `pmNotes` | [Optional] Notes for the project managers
`this.submitter` | `submitter` | [Optional]  Set the submitter to a user other than the logged in user
`this.ticket` | `submissionTicket` | Submission ticket
`this.workflow` | `workflow` | [Optional]  Set the PDWorkflow to use


## Document

```javascript
class PDDocument {
	constructor() {
		this.data = '';
		this.name = '';
		this.sourceLanguage = '';
		this.targetLanguages = [];
		this.clientIdentifier = null;
		this.encoding = "UTF-8";
		this.fileformat = '';
		this.instructions = '';
		this.metadata = null;
	}
	getDocumentInfo(submission){}
	getTargetInfos(submission){}
	getResourceInfos(){}
}
```

Document containing textual elements that require translation.

Method | Returns | Sumary 
------ | ------- | -------
`getDocumentInfo` | Object | Internal method used by the UCF API
`getTargetInfos` | Array | Internal method used by the UCF API
`getResourceInfo` | Object | Internal method used by the UCF API

Method | Parmeter | Sumary 
------ | ------- | -------
`this.data` | `data` | Document data for translation
`this.clientIdentifier` | `UID` at client's System | Specify a unique identifier for this document (if one exists) in the content management system
`this.ncoding` | `encoding` | Set encoding for the document to be uploaded
`this.fileformat` | `fileFormat` | If not set, defaults to configured `fileFormat` on project director
`this.instructions` | `instructions` | Additional metadata that you may want to attach to your document
`this.sourceLanguage` | `sourceLanguage` | Set Source Language of the document that requires translation
`this.targetLanguages` | `targetLanguages` | Set target languages into which this document will be translated
`this.metadata` | `metadata` | Set metadata for the document being submitted for translation

## Workflow

```javascript
class PDWorkflow {
	constructor(externalWorkflow) {
		this.name = externalWorkflow.name;
		this.ticket = externalWorkflow.ticket;
	}
}
```

Method | Returns | Sumary 
------ | ------- | -------
`this.name` | String | returns the workflow name set up in PD
`this.ticket` | String | returns the workflow ticket for a given `WorkflowName`

## LanguageDirection

A language direction defines the source and target language direction for which users can submit translation requests.

```javascript
class PDLanguageDirection {
	constructor(externalLanguageDirection){
		this.sourceLanguage
		this.sourceLanguageName
		this.targetLanguage
		this.targetLanguageName
	}
}
```

Method | Returns | Sumary 
------ | ------- | -------
`this.sourceLanguage` | String | gets the source language ISO code [e.g. en-US] configured in PD
`this.sourceLanguageName` | String | gets the source language name [e.g. English (United States)] configured in PD
`this.targetLanguage` | String | gets the target language ISO code [e.g. de-DE] configured in PD
`this.targetLanguageName` | String | gets the source language name [e.g. German (Germany)] configured in PD


## Target

```javascript
class PDTarget {
	constructor() {
		this.clientIdentifier;
		this.documentName = ''
		this.documentTicket = ''
		this.sourceLocale = ''
		this.targetLocale = ''
		this.metadata = []; // [{key1: value1}, {key2 : value2},...]
		this.ticket = '';
		this.wordCount = undefined;
	}
	initializePDTarget(externalTarget) {
		this.documentName
		this.sourceLocale
		this.targetLocale
		this.ticket
		this.documentTicket
		this.wordCount
		this.clientIdentifier
	}
}
```

Target is the equivalent of the translated version of a Document. A Document can be submitted for translation into one or more languages. For each language, a target is generated on project director. E.g. If 2 documents were submitted for translation into 5 languages, upon completion of the translation workflow, you would have (2 x 5=) 10 targets available for delivery.

Method | Returns | Sumary 
------ | ------- | -------
`this.clientIdentifier` | String | `UID` configured by client when the document was sent
`this.documentName` | String | Document Name of the document that was submitted for translation
`this.documentTicket` | String | Document Ticket of the source document that was submitted for translation
`this.sourceLocale` | String | Source locale (language ISO code) for this target
`this.targetLocale` | String | Locale (language ISO code) for this target
`this.metadata` | Hash | Metadata in the form of key-value pairs set on the document that was submitted for translation
`thia.ticket` | String | Target Ticket
`this.wordCount` | String | Amount of words for this target 



## WordCount

```javascript
class WordCount {
	constructor() {
		this.golden = 0; // int
		this.exact_100 = 0; // int
		this.fuzzy = 0; // int
		this.repetitions = 0; // int
		this.nomatch = 0; // int
		this.total = 0; // int
	}
	initializeWordCount(golden, exact_100, repetitions, nomatch, total) {
		this.golden = golden;
		this.exact_100 = exact_100;
		this.fuzzy = total - golden - exact_100 - repetitions - nomatch;
		this.repetitions = repetitions;
		this.nomatch = nomatch;
		this.total = total;
	}
}

```

Please note that words are counted and analyzed per segment against a Translation Memory, TM. A TM is a database that stores segments, which can be sentences, paragraphs or sentence-like units (headings, titles or elements in a list) that have previously been translated, in order to aid human translators. The translation memory stores the source text and its corresponding translation in language pairs called “translation units”. Individual words are handled by terminology bases and are not within the domain of TM.

Method | Returns | Sumary 
------ | ------- | -------
golden | int | Total amount of words considered to be 100% matches in context against the TM (either because of previous and next segment match or due to attribute)
exact_100 | int | Total amoutn of words considered to be 100% matches against the TM
fuzzy | int | Total amoutn of words considered to be partially matching (99%-75%) against the TM
repetitions | int | Total amount of words that match in the same document
nomatch | int | Total amount of words that do not match (75%-0%) against the TM
total | int | Total amount of words counted on the document (`total` = `golden` + `exact_100` + `fuzzy` + `repetitions` + `nomatch`)

## CustomAttribute

```javascript
class PDCustomAttribute {
	constructor(customAttribute) {
		if (customAttribute) {
			this.mandatory = customAttribute.mandatory;
			this.name = customAttribute.name;
			this.type = customAttribute.type;
			this.values = customAttribute.values;
		}
	}
}
	
```

Each project can be configured to allow users to specify custom attributes for their submissions. The custom attributes can be either plain text, or an enum. Custom attributes can be used for reporting, billing etc. eg. You could specify a Cost Center as a custom attribute and instruct your account manager to invoice by cost-center

Method | Returns | Sumary 
------ | ------- | -------
`mandatory` | Boolean | Defines if the attribute is mandatory or not
`name` | string | Prints the attribute name
`type` | string | Prints the attribute type
`values` | Array | Prints the attribute value(s)

# Configuration Classes

## PDConfig

```javascript
class PDConfig {
	constructor(configObj) {
		this.url = configObj.url;
		this.username = configObj.username;
		this.password = configObj.password;
		this.proxyConfig = configObj.proxyConfig;
		this.userAgent = configObj.userAgent;
	}
}
```

Project director configuration

Method | Parmeter | Sumary 
------ | ------- | -------
`this.url` | `url` | Set the URL of the project director server to connect to
`this.username` | `username` | Sets Project Director username
`this.password` | `password` | Sets Project Director password
`this.proxyConfig` | `ProxyConfig` | Set proxy configuration for outbound traffic
`this.userAgen` | `userAgent` | Sets the user Agent for this client. A user agent is any user friendly identifier which is used to categorize the logging on GlobalLink


## Proxy Config

```javascript
class ProxyConfig {
	constructor() {
		this.proxyHost = '';
		this.proxyPort = '';
		this.proxyUser = '';
		this.proxyPassword = '';
	}
}
```


Method | Parmeter | Sumary 
------ | ------- | -------
`this.proxyHost` | `proxyHost` | Set Proxy server IP address or hostname
`this.proxyPort` | `proxyPort` | Set Proxy server port
`this.proxyUser = '';` | `proxyUser` | Set Proxy user
`this.proxyPassword = '';` | `proxyPassword` | Proxy server user password
