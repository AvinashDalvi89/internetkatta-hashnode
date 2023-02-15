# Take a look at Amazon Kendra

Hello Devs,

In this blog going to walk through another amazing AWS service which was launched recently in Amazon AI Conclave, Bangalore 2022.

## Amazon Kendra

![Amazon AI Conclave Key note](https://cdn.hashnode.com/res/hashnode/image/upload/v1673402041197/8f0f35b6-4f07-4fbb-8140-720dbf397ce1.jpeg align="center")

### Let's understand why Amazon Kendra.

Finding accurate answers quickly, with ML-powered intelligent search

In a traditional search like an elastic search whenever you search for one question the algorithm tries to divide the question into keywords and tries to find out all keywords in the available data. Look at the below image. The major issue was having the right answer or can say right link or place to get an answer was a little challenging with traditional search.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673674896174/55b4e407-f336-4605-b555-88af3c1eb85d.png align="center")

Here is Amazon Kendra is helping you to get to the right place to the point where exactly the answer is. Amazon Kendra allows users to search unstructured and structured data using NLP ( Natural language processing ) and advanced search. Giving users an experience exactly like they feel talking human expert. Take the example of the question "**Where is the IT desk in the company?** " For this in traditional search will get a list of links which have the word "**IT**", "**Desk**" "**Company**" etc. But Kendra will highlight directly to a page where it mentioned it is on "**1st Floor**"

Let's check out what makes Kendra a powerful search.

### What is behind Kendra?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676384823183/0db0bb49-021b-4093-a359-32b5463963b8.png align="center")

Kendra uses data sources like S3 buckets, Salesforce or Slack or any other custom data sources as a connector. At times you can connect multiple connectors. Create an index from all data sources and it will be an updatable index of documents of a variety of types like structured text and unstructured text. Below is a list of unstructured text format support.

* HTML files
    
* Microsoft PowerPoint (PPT) presentations
    
* MS WORD documents
    
* Plain text documents
    
* PDFs
    
* Comma Separated Values (CSV) files
    
* Microsoft Excel (MS EXCEL) files
    
* XML files
    
* JSON files
    
* Markdown Documentation (MD) files
    
* Rich Text Format (RTF) files
    
* Extensible Stylesheet Language Transformation (XSLT) files
    

Once it creates an index from the data source next step is to add documents directly to it or from a data source. The last step will be to use Kendra API to use for search across data.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676424183311/ce55e9ce-801f-44e5-bada-2cf2e5ab6beb.png align="center")

One example of Kendra API :

```python
import boto3
import pprint

kendra = boto3.client("kendra")

# Provide the index ID
index_id = <index-id>
# Provide the query text
query = "search string"

response = kendra.query(
        QueryText = query,
        IndexId = index_id)
```

Kendra also supports other configurable parameters that will help format the response and how you want the result to look, such as 

* Querying an index
    
* Tabular search for HTML
    
* Browsing an index
    
* Filtering queries
    
* Filtering on user context
    
* Query responses
    
* Query suggestions
    
* Query spell checker
    
* Tuning responses
    
* Sorting responses
    
* Response types
    

### What are use cases?

* Enhance internal search experiences for employees
    
* Improve customer interactions like a chatbot or call centre
    
* Integrate search into SaaS applications like CRM, Salesforce
    

### Prerequisites for starting Kendra :

* AWS account if you have one else you need to create a new AWS account
    
* Under the free trial, AWS provide 750 hours free.
    
* Familiar with AWS Console, AWS CLI or Boto3 for Python or other languages SDK
    

### What are the connectors provided by Kendra :

The list is big you can check this link [https://aws.amazon.com/kendra/connectors/](https://aws.amazon.com/kendra/connectors/)

### Features :

* Deploy Amazon search UI in few clicks via [Amazon Kendra Experience Builder](https://docs.aws.amazon.com/kendra/latest/dg/deploying-search-experience-no-code.html)
    
* Intelligent search
    
* Incremental learning
    
* Tuning and accuracy
    
* Connectors
    
* Domain optimization
    
* Search Analytics Dashboard
    
* Custom Document Enrichment
    
* Query auto-completion
    

### Pricing :

You can start a free tier account for 750 hours for the first 30 days. Connectors usage doesn't come under free usage.

**Note**: Charges will be applied on a per-index basis. Charges apply even if the provisioned indexes contain no documents and no queries are executed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676424868427/949b28fb-cb17-49fa-a4c5-362dc2f631f2.png align="center")

more details about pricing each use case-wise can be found here [https://aws.amazon.com/kendra/pricing/](https://aws.amazon.com/kendra/pricing/)

I hope this blog helps you to learn. Feel free to reach out to me on my Twitter handle @[@AvinashDalvi_](@AvinashDalvi_) or comment on the blog.

Keep learning and keep sharing!

### References :

* [https://aws.amazon.com/kendra/](https://aws.amazon.com/kendra/)
    
* [https://docs.aws.amazon.com/kendra/latest/dg/what-is-kendra.html](https://docs.aws.amazon.com/kendra/latest/dg/what-is-kendra.html)
    
* [https://docs.aws.amazon.com/kendra/latest/dg/deploying-search-experience-no-code.html](https://docs.aws.amazon.com/kendra/latest/dg/deploying-search-experience-no-code.html)
    
* [https://aws.amazon.com/kendra/features/](https://aws.amazon.com/kendra/features/)