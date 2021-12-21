# Google Cloud Platform

- Global model.
- Designed for developers.
- Physical infrastructure
  - Data center totally s`eparated from the services.
  - They are totally unconnected.
  - A zone is one or more data centers connected. Same concept, independant (water, elecitricty, bandwith)
  - Zones in the same region communicate super fast.
  - MultiRegion.
  - Connected by a private global network (AKA connected one with the other.. Privately.)
  - Pop Points of presence. Network edges and CDN

Google routes so traffic enters from internet ad edge closeset to source.

- Single global UP address can load balance worldwide

## Pricing

- Provisioned
- Usage
- Network traffice. Free on the way in.
- Charged on the way out by GBS.
- Egress to GCP services sometimes is free.

## Security

- Separation of duties
- Everything is always encrypted at rest
- Strong key and identity management
- Everything is encrypted. WAN traffic too. They are even moving towards encrypting local traffic between data centers.
- Philosphy: distrust the network.
- BeyondCorp. Allows secure connections without VPN.

## Scale and automation

- Scalability must be unbounded
- Automate all the things

## Resource Quotas (soft limit)

- Scope
- Changes. Automatic or by request

## Projects

- Projects are similar to aws account
- Projects own resources
- resources can be shared with other projects
- projeces can be grouped and controlled hierarchily.

## COMPUTE

### Compute Engine (GCE)

- Like Ec2
- Fast booting Virtual machines, you can rent on demand
- Fast bootup (35 seconds)
- Infrastructura as a service
- You can choose CPU/RAM. There are pre sets
- Pay by the second, minimum one minute.
- Cheaper if you keep running it. 'Sustauned use discount'
- Flexible.
- Even cheaper for preemptible (like spot market on AWS without being an auction) or long-term use commitment in a region (like flexible AWS reserved instances)

### Kubernates engine GKE

- Managed Cluster for running docker containers (with autoscaling)
- Used to be called Google Container Engine until Nov 2017
- Comparable to EC2 Container
- Kubernates DNS on by default for service discovery
- NO IAM integration (unlikw AWS ECS)
- Integrates with persistan Disk for storage
- Pay for underlying GCE instances
- Production cluster should have 3+ nodes
- No GKE management fee, no matterhow many nodes in cluster, as Nov 2017

### Google App encine (GAE)

- Platform as a service
- Like Heroku, Elastic Beanstalk
- Paas
- Compute, storage, ques, NoSQl
- Can run containers, Node, Python, Go
- Auto Scales on load
- Can auto scale to 0 if no traffic

### Cloud functinos

- Runs a code in response to an event. Node, Python, Java, go
- Functions as a service. Serverless
- Pay for cpu and ram assigned to function, per 100 ms;
- Each function gets an http endpoint
- Can be triggered bt GCS objects, pub/sub messages, etc
- Massively scalable (horizontally)/ Runs many copies when needed.
- Often used for chatbots, message processors, IoT, automation, etc.

## Storage

- Always the data in encrypted at rest.

### Local SSD

- Very fast 375 solid state drivers physically attached to the server.
- High performance.
- All data will be lost when the instance shuts down. But can survive a live migration.
- All data at rest, always encrypted.
- Pay by gb-mongth provisioned.

### Persistant disk

- Blobk based network attached storage. Boot disk for every GCE instance.
- Slower than the previos, but still pretty fast.
- Persistant! and replicated for durability.
- Can resize in use.
- Possibility to take 'snapshots'
- Snapshots are available globally.
- It is block based.
- Pay per GB depending on performing class, plus snapshot GB/mo used
- Good for multiple clients reading data

### Filestore

- Filetype storage. Comparable to Elastic File System, or a NAS.
- Fully managed file based storage
- Performance is predictable.
- Main usage: migration to cloud ("lift and shift")
- Fully manages file serving, but not backups.
- Pay por provisioned TBs in "Standard" (minimum 1tb) or "premium" (minimum 2.5 tb)
- Premium has higher IOPS
- Not enough IOPS? Try Google Cloud Storage

### Cloud Storage

- Stores info in a bucket.
- Eleven 9s of durability
- Strongely consisten.
- Integrated site hosting and CDN functionality.
- Multiregional, Recional, Nearline (less than once a month), Coldline (access less than once a year)
- Can be put on auto choose mode.

## DATABASES

### CLOUD SQL

- MySQL and PostreSQL databases.
- Supports automatic repliaction, backup, failover.
- Scaling is manual. Both vertically and horizontally.
- Effectively pay for underlying GCE instances and Persistant Desks (PDs
- PLus some baked in fees

### Cloud Spanner

- First horizontally scallabe, sontrgly consisten, relational database service
- Like Sharded mySQL, Cockroach DB
- Can scale from 1 to hundred or thousands of nodes
- A minimum of 3 nodes is recommended for production evironemnts.
- Chooses consitency and partition tolerance
- High availability: 99.999 (5 9s)
- Not for vailover
- Pay for provisioned node time, plus sued sotarge-time.
- Not for your regular barbaque app

### Big Query

- Serverless colum store data warehouse for analytics using SQL (like AWS redshift)
- It is serverless.
- It scales internally. Athena is the copy of BigQuery
- Attempts to reuse cached results, which are free.
- Pay for fbs acually considered during queries
- Pay for data stored (gb-months)
- Cheaper when table not modified for 90 days

### No SQl. Cloud Bigtable

- like DynamoDB
- Low alatency & high trhouhput for large operational & analytics apps
- Scales without breaking functionalty.
- Storage autoscales
- Processing nodes must be scalled manuallyPay for processing node hours
- Pay for GB hours used for storage.
- Used for HUGE worload

### Cloud Datastore

- managed & autoscaled noSQL db with indexes, queries, and acid transaction support
- NoSQL. so queries can get complicated.
- No joins or aggregation, and must line up with indexes.
- Similar to MongoDb

### Realtime DB & Cloud Firestore

- Sockets
- NoSQL ddocuntes sotres with realtime client updates via managed websockets.
- Firebase in single potentially huge JSON doc, localte only in central US
- Firesotre has collections, documents and contained data
- Free tier, fla, or usage-based pricing (Blaze)
- Firestore: multi regional
- Firebase Realtime DB: Zonal

## Large Data transfer

### Data Transfarcer Appliance

- Rackable high capacity storage server to physucally ship data to GCS
- Ingest only, 100Tb or 480TB version

### Storage Transfer Service

- Copies objects for you
- Destination is awlways GCS bucket
- Source can be S3, Http/ Https endpoint, or another GCS bucket
- One time or scheduled recurring transfers
- Free to use, but you pay for its actions
  
## External networking

### Google Domains

- Googles registrar for domain names. Like GoDady, route53.
- Private Whois Records
- Built in dns or point to yown custom name servers
- Email forwarding with automatic setup

## CLOUD DNS

- Saclabale reliable, managed authoritative domain name system service.
- 100% uptime guarantee
- Public and private managed zones.
- DNS accessed only for your own private VPC
- Low latency globally
- Manage your records through UI, CLI, API

### Static IP

- Reserve static IP addresse in prjects and assign them to resources
- Regional IPs used for GCE instances & network Load balancers
- Global IPs used for blogal load balancers
- HTTP, SSL proxy, and TCP proxy
- Anycast Up simplifies DNS
- Pay for reserved IP not in use, to discourage wasting resources

### Load balancing

- Like elastic load Balancing. (link NGINX)
- High performace, scalabe traffic, fistrubtion integbrate with autoscaling & cloud CDN
- Naturally handles spikes without any prewarming
- Helath checks, round robin, session affinity.
- Forwarding rules based on Ip, protocol, port
- Regional and global load balancers.
- Global load ablancers with multi region failover for http(s), ssl proxy & TCP proxy
- Reacts quickly (unlike DNS) to changes in users, traffic, network, health, etc
- Pay by making ingress traffic billable

### Cloud CDN

- Low latency content delivery netwoerk based on HTTP (Amazon CloudFront, Akamai)
- Doesn't support custom origins (GCP only)
- Simple checkbox on Https load balancer config turns this on
- Depending on POP.
- Pay fot http volume. Client egreess charges, depending on location
- Pay is complicated

## Internal Networking

- Data around your system.

### VPC (virtual private cloud)

- Global IPV4 unicast sotware defined network (sdn) for Gcp resources
- Private cloud
- Automatic mode is easy, custom mode gives control
- Configure subnets., routes, firewalls, VPNS, BGP, etc
- VPC is global and subnets are regionalt (not zonal!)
- Can enable private access to some GCP
- Free to configure VPC
- Pay to user service and for network egress

### Cloud Interconnect

- Connect external network to Google's network
- Private connections to VPC via cloud VPN or dedicated / partner interconnect
- Significally lower egress fees
- Direct peering for high volume, if met requirements.
- Otherwise by a partner for lower volume (more expensive)

### Cloud VPN

Vpn to connect to VPC via public internet for low volume data connections

- For persistant, static connection between gateways
- Peer VPN gateway must have static unchaning ip
- Ecnrypted to VPC (as opossoed to dedicated Interconnect), into one subnet
- 99.9% availability
- Supports both static and dynamic routing
- Pay per tunnel hour

### Dedicated interconnect

High volume data connection
Vlan is private connection to VPC is one region, no publi

to keep on

## MACHINE LEARNING

## ML learning

Massivelay scalabele managede service for training ML models y makin predictions
Enables apps devs to use tensroflow on datasetes of anzy size, endless use cases
Integrates with GCS BQ, CLoud dtatalab, cloud dataflow (preprocessing
Prediction on real tie or batch
Download the models and make predictions anywhere ,desltop, mobile own server
Hyperyune
Training. Pay per hour depending

### CLOUD Vision API

- calissifies images into catefories. detects objects, faces, fnds and rads irnpted text
- pre trained ML model to analuxr images
- Pay per image, based on detection features requeste.
- Higher price for OCR or full documents.

### Cloud speach api

- Automatic speech recognition spoke word audio files into text
- Pre trainedl ML model for reginzing 110 + languages
- Pre recoreded or real time audio, stream results in real time.
- Enables boice command and control and transcribin user microphone dictations
- Handles noise source audio
- Filters inappropiate content in some lenguage
- Pay per 15s of usage  

## Cloud Natural Language API

- ANALYZES FOR SENTIMENT, INTETN, CONTENT CLASsification, extracs info.
- Excelent with speech API, Visio API, translation API
- Extracts token, senteces, parts of speech and dependency trees
- Analysis finds people, places, things, labels them, links to wikipedi
- Price depending of request. pay per 1000 requests

## Cloud translation API

- Translate over 100+ languages, optionally auto detects source language.
- Pre trained ML model for recognizing and translating semantics, not just syntax.
- Combine with spech, vision, natural language API for powerful workflows
- Text or hTML
- pay per character processed information

### DialogFlow

Build conversatoinal interfaces for websites, mobile apps, messagin, IOT devices
Pre trained ML model and service for accpeint , lexing input & responsing
Enables useful chatbots
Tarain it to indetiy custom entity typres
Pay per interactions

### Cloud video intelligence API

- Video scene analysis and subject identification
- Lable detection: like dog, flower, car
- Shot change detection
- SafeSearch detection. adult content
- Search in video like in text documents.
- Regions for regulatory compliance.
- Pay per minute of video processed

### Cloud job discovery

- Helps career sites, company job boards
- Show me jobs within a 30 minutes commute from my job
- Integrates with many job hiring
- Lots of features, suach as commute distance, and recognizing jargon adn abreviations

## Big DATA and IOT

- 4 different stages of data: ingest, store, process & analyze, explore & visualize

- Ingest: App engine, compute engine, container engine, cloud pub/sub, stracdrivver logging, cloud transfer service
- Store: Cloud Storage, Cloud SQL, cloud datastore, cloud bigtable, bigquery
- Process & Analyzr: Cloud dataflow, cloud dataproc, biquery, cloud ML, cloud vision API, cloud Speech API, trasnlate API, cloud natural lang api
- explore and visualize: Cloud datalab, google data studio, google seets

### Cloud IT Core

- When data is created. ingest data from IoT devices.
- Handles device identity, authentication, config and control
- Connect using MQTT or HTTPS protocol
- CA signed certicates can be used to verify device ownershup on first connect
- Register device when it comes online!
- Able to make control changes even when offline (push on online)

### Cloud  Pub/Sub

- infinitely scablable at least once messagin for ingestion decoupling, etc.
- Glue everything together
- Message => send to a topic => Subscribers from the topic receives the messages
- Global by default with consistence latency.
- Up to 10mb, undeliverd ones stored for 7 days, but no DLQ
- Push mode delivers to https & succeeds on HTTP success status code
- Pay per data pushed

### Cloud dataprep

- Visually explore, clean and prepare data for analysis without running servers.
- For business analysts
- Not managed by Google, but by Trifacta
- Detects schemas, datatypes, possible joins, anomalies
- Pay for underlyin dataflow plus management overhead
- Uses Machine learning to suggest transformations.

### Cloud dataproc

Batch mapreduce processing, managed spark and hadoop clusters

???
best for moving existing spark hadoop to GCP

### Cloud Dataflow

- Use if you are not using Hadoop or Spark.
- Smartly autoscaled & fully managed batch or stream mapreduce like processing.
- Autoscales, dinamically redistrivutes lagging work
- Pay for underlying worker GCE
- Pay per second for CPUS, ram,e tc

### Cloud Datalab

- Interactive tool for data exploration, analysis, visualization and machine learning
- Uses Jupyter Notebook
- Pay per the instance storing your notebook

### Cloud data studio

- Visualization tool for dashboard and reporting
- Meaningful data stories / presentations
- Pay per the services you access, not data studio

### Cloud Genomics

- Store and process genomres and related experiments
- Query complete genomic information of larce research projects in second
- Many genomes and experiment in parallel
- Supports requester pays sharing

## Identity and access (Core security)

- Triple AAA model: Authentication, Authorization, Accounting (what did they do and when)

### Roles

- Collections of permissions to use or manage GCP resources. Like policies in AWS
- Permissions allow you to perform certain actions: Service.Resource.Verb
- Primitive roles: Owner, Editor, Viewer (read only)
- Predefined Roles. Granular acces. roles/bigquery.dataEditor
- Custom Roles: Project or org level collection

### CLOUD IAM

- Controlls access to GCP resources, authorization, not really authentication, identity
- Membes is user, group domain, service account,
  - Individual google account, Google group, G suite / Cloud intity domain
  - Service account, belongs to an application or instance rather than an individual end user
  - Every identity has a unique email
- Policies bind memebers to roles at a hierarchiy level.
- Answers the question: Who can do what to which things
- Free to use

### Service account

- Represents an application, not an user.
- Can be assumed by individual users or assumed applications
- Use service account rather than user account or API keys.
- Use least privilege
- Recommended: Cloud Platform manged keys are better
  - No direct downloading: Google manages the keys and rotation (1 per day)

### Cloud Identity

- Inditity as a service to provision and manage users and groups
- Free google accounts for non g suite users, tied to a verified domain
- Centrally manage all users in google admin console
- 2 step verification, and enforcement.
- Sync from active directoryt and LDAP directories via google cloud directory sync
- Free to use,

### Security key enforcement

- Usb or bluethoot 2 step verification device that prevents phishing

### Resource manager$$

- Centrally manage and secure organizations project with custom folder hierarchy
- Folders per your business needs
- Recycle bin, allows undeleting project
- Apply IAM policies

### Cloud IAP (identity Aware proxy)

- Guards apps running on GCP via identity verification, not VPN access
- Only passes authed reques through (similiar to amazon api GetAway)
- Relatively straightforward
- Only pay for load balancing.
  
### Cloud audit logging

- Who did what where and when. Similar to cloudtrail.
- Tracks logs
- Can't be deleted or changed.
  - Types:
  - Admin activity and system event (400 day retention)
  - Access trasnparency
    - Shows action by google support staff
  - Data access (30 days)
- Data access logs pricer through stacdriver logging, rest are free

## Security Managemet - Monitoring And Response

### Cloud Armor

- Edge leven protection from DDos & other attachs on global HTTP(s) LB (like aws shield and aws firewall). Global
- Coordination across all the network edges
- Offload work: blocked attacs never reach your systems
- Detailed request level logs in Stackdriver logging
- Individual or ranges of IPS for disallow / allow (block, allow)
- More intelligent rules forthcomint XSS, geobased, SQLi
- Preview effects before making them live
- Pay per policy and rule configured, plus for incoming request incomint to your system

### Security scanner

- Similar to Amazon Inspector, Trustwave app Scanner
- Free but limited GAE app unleraibiility scanner with very low false positive rates.
- Follows links, and attemps to execirse as many user inputs and event handlers as possible
- Can identify CSS
- Flas injection
- Mixed content
- Outdated/insecure libraries

### Cloud DLP APi

- Data loss api
- Finds and optionally redacts sensitive info in unstructured data streams
- Similar to AWS macie
- Help you minimize what you collect, expose or copy to other systems.
- Usefull for senstive data: credit card, social security numbers, passports, etc.
- Scans text and images
- Pay for amount of data processes per gb
- For streaming is still a mess.

### Event Threat Detection

- Tracks your Stackdruver logs for suspicious activity
- Like Amazon GuardDuty
- Quickly detects many possible threats, including
  - Malware, cryptomining (cpu usage), outgoing DDos Attachs, port scanning, brute-force SSH
- Detects that a system is trying to overwhelm a port, an input, etc.
- Unauthorized GCP via abusive IAM access
- Can exported pased logs to BigQuery for forensic analysis
- Integrates with SIEMs like google's cloud SCC
- No charge for ETD, but charged for what it does

### Cloud SCC (security command center)

- Comporehensive security management and data risk platform for GCP
- SIEM Security information and event management software
- Helps your prevent, detect, & respond to threats from a single pane of glass
- Use 'security marks' to group, track, and manage resources
- Integrates with ETD, cloud scanner, DLP and external sources
- Can alert and export data
- Free! But charged for the services used.
- Could be also be charged for excessive uploads of external findings

## Encryption key management

### Cloud KMS

- Low latency service to manage and use cryptographic Keys
- Supports symmetric and assymetric algorithms
- Moves secretes out of code, and into the environment
- Ecnrypt and decrtypts keys
- Allows key rotations, still keeping old keys versions
- Key deletion has a 24 hs delay
- Pay for active key versions stored over time
- Pay for encrypting and decrypting

### Cloud HSM

- Cloud KMS keys managed by FIPS 140-2 Level 3 certified HSM
- Device hosts encryption Keys and performs cryptographic operations
- Enables to meet compliance that mandates hardware environment
- A feature of Cloud KMS
- Same API, features, IAM integration
- Pay per active key versions, and key operations
- Priced like cloud KMS, but more expensive on asymetric keys.

## Operations and management

### Stackdriver

- Family of services, monitoring logging & diagnosing apss on GCP / AWS / Hybrid
- Deeply integrated with GCP
- One stackdriver account can track multiple multiple projects: GCP projects, aws accounts, other resources.
- Simple usage based pricing

### Stackdriver Monitoring

- Like Datadog
- Gives visibility into performance, uptime, & overal health of cloud apps
- Custom metrics, dashboards, global uptime monitoring, alerts
- Cross Cloud
- Multicondition rules & resource organization
- Alert via email, slack,, mobile app, SMS, etc
- Automatic GCP / Anthos metrics always free
- Pay for API calls & per MB for custom AWS metrics

### Stackdriver Logging

- Compare to CloudWatch Logs
- Store search, analyze, monitor, and alert on log data & events
- Real time metrics from log data, then alert or chart them on dashboard.
- Real time log data to bigQuery and SQL like querying
- Pay per GB ingested & stored for one month, first 50gb per month free
- Cloud Audit logging access transparency logs also free

### Stackdriver Error reporting

- Counts analyzes, aggregates and tracks crashes in helpful centralized interface
- Aggregates errors into meaningful gruops
- Instantly alers when a new app error cant be grouped with existing ones
- Link directly from notification to error details: tie chart, occurances, etc
- Knows Java, Python, JS, Ruby, C#, PHP, Go

### Stackdriver Trace

- Tracks and displays call tree & timings across fistributed system, to debug perf
- Capture traces from Google App Engine
- Trace APi and SDKS for ava, Node, Ruby, Go
- View aggregate app latengcy info or dig into individual traces to debug problems
- Detects app latency shift
- Pay for ingsting and retrieving trace spans

### Stackdriver Debugger

- Grab program state (callstace, variables, epxressions) in live deploys, low impact
- Source view supports cloudr source repository, github, bitbucket, local, && upload
- Java and python on GCE, GKE, and GAE (std and flex)
- NodeJS on GCE, GKE, Gae Flex
- Share debugging sessions to coworker
- free to use

### Stackdriver profiler

- Continous CPU and memory profiling to improve perf & reduce cost
- Low overhead, so use in production too! (0.5% typical, max 5%)
- Agent-based needs to be installed
- Saves profiles for 30 days
- Can download for longer term storage
- No charger

### Cloud deployment manager

Create/manage resoures via declarative tempates: Infrastructure as code
Declarative allows automatic parallelization
Templaes in YAML, PYTHON, Jinja2
Supports input and output, with JSON schema
Can preview deployment
Free service, just pay per resources

### Cloud billing API

- Manage billing for GCP projects and get GCP pricing
- Billing config
  - list billing accouts, get details and associated projects for each
  - Enable, disable, or change projects billing account (associate or disassociate)
- Pricing:
  - List billeable SKUs, get public pricing for each
  - get SKU metadata like regional availability
  - Eport of current bill gto GCS or BQ is possible, but not through API.

## Development & APIS

### Cloud Source repositoroes

- Hosted private git repositories, integrations to GCP, and other hoster repos (like github)
- Supports git.
- No pull requests
- No enhanced workflow support like pull requests
- Can set up automatic sync from github or bitbucket
- Natural integration with Stackdriver debugger for live debugging deployed apps
- Pay per project-user active each month
- Pay per GB month of data storage
- Pay per GB of data egress

### Cloud Build

- Turns source code into a build package, like a docker image.
- Continously builds, tests and deploys. CI/CD service
- Like Amazon CodeBuild, or Travis CI
- Trigger from cloud Source repository or zip in GCS.
  - Can trigger from Github and BitBucker via Cloud Source repositores Reposync
- Runs many builds in parallel (10 at a time)
- With dockerfile: super-simple build + push - Plus scans for package vulnerabilities
- Pay per time.
- JSON/TAML file: flexible & Parallel Steps
- Maintains build logs and build history
- Pay per minute of build time - free until 120 minutes per day

### Container Registry

- Fast, private, docker image storage. Based on GCS with dovker V2 Registry API. Comparable to Docker Hub
- Creates & manages a multiregional GCS bucket, tren translates GCR calls to GCS
- IAM integrations simplifies builds and deployments within GCp
- Quick deploys because of GCP networking to GCS
- Directly compatible with standard Docker CLI, native Docer Login Support
- UX integrated with Cloud Build & Stackdriver logs
- UI to manage tags and search for images
- Pay directly for storage and egress of underlying GCS(no overhead)
- Lightway overlay of GCS

### Cloud endpoints

- Exposes what is running on an API
- Handles authorization, monitoring, loggint, & API keys for apis baked by GCp
- Based on NGINX
- Proxy instances are distributed and hook into Cloud Load Balancer
- Super fast. Extensible Service Proxy container based on NGINX. less than a 1ms per call. Super fast.
- Uses JWTs and integrates with Firebase, Auth0, & Google Auth
- Integrates with Stackdriver Logging and Stackdriver Trace
- Extensible Service Proxy can transcode HTTP/JSON to gRPC
  - Api needs to be resource oriented (RESTful)
- Pay per call to your API

### Apigee

- Api management platform for whole API lifecycle.
- Can change calls between different protocols: SOAP, REST, XML, binary, custom
- AUthenticate via Oauth //SAML/LDAP, authorize via role-based acccess control
- Throttle trafic with quotas, manage API versions, etc
- Apigee Sense identifies suspicious behaviour
- Apigee API monetization spuport varios revenue models / rate pans
- Enterprise ter, Contact Sales

### Test Lab for Android

- Cloud INfrastructure for running test matrix across variety of real Andriod Devices
- Mobile testing
- Production grade devices flashed with android version and local you specify
- Robo test, captures log files, saves screenshops and video to show steps
- Can run expresso and UI automator 2.0
- Pay as you go, Also by Plans (Spark, Flames)

## Api Hosting
