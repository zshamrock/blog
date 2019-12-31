{:title "Designing a scalable system"
:layout :post
:tags ["craft" "distributed"]
}



Check the email I've sent to Clint.
Things to cover: when to use what, common bottlenecks (CPU, Network, and I/O?)
Numbers: how much the relational database can handle, types of distributed systems/replication models (master-slave, consistent hashing).

How you solve scalabality?
Partitioning!
well known techniques to achieve scalability and availability: Data is partitioned and replicated using consistent hashing [10], and consistency is facilitated by object versioning [12]. (Dynamo paper)
The main contribution of this work for the research community is the evaluation of how different techniques can be combined to provide a single highly-available system

Where which tool is most suitable.
Notes on real-time analytics (available tools on the market)

Designing from web till backend.

Does the language matter?
How different languages can help?

DL it runs on Tomcat, powered by MSSQL


