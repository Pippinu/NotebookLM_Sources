# Suggested New Exam Questions for CLC_Oral_Exam_Prep_Ans.pdf

## Lesson 1: Cloud Computing

* Which are the business drivers for cloud computing? (e.g., Capacity Planning, Cost Reduction, Business Agility) [cite: 2, 50]
* Suppose that you would like to deploy a web application composed by: a frontend server to receive http requests and produce http reply/answers, a business logic server to build the content of the web pages; a database server, used by the business logic to extract information. Suppose also that the business logic component should scale (horizontally). Which system architectural style/model could be used for the deployment? (Consider options like Multi-tier, Microservice, SOA) [cite: 6, 7, 94, 95, 96, 97, 98, 194, 195, 196, 197, 198, 352, 353, 354, 355]
* In what differ microservices and the SOA? (Consider aspects like communication mechanisms, data handling, size of services, coupling) [cite: 9, 52, 277, 409, 410, 411, 452, 453, 454, 455, 456]
* The NIST definition of Cloud computing mentions that "Computing resources are pooled to serve multiple consumers using a multi-tenant model". What does multi-tenancy mean? [cite: 161, 162, 163, 164, 165, 447]
* Which of the following definition of capacity planning is correct? (e.g., a methodology to determine the right amount of capacity when needed to avoid Over-provisioning and Under-provisioning) [cite: 83, 84, 85, 86, 87, 173, 174, 175, 429, 430, 431, 432]
* What is a microservice architecture? (e.g., an architectural style structuring an application as a collection of loosely coupled, independently deployable services organized around business capabilities) [cite: 88, 89, 90, 91, 438, 439, 440, 441]
* Which of the following are challenges in the microservice architecture? (e.g., Finding the right set of services, developing/testing/deploying complex applications, efficient resource management, orchestration, choreography) [cite: 92, 93, 187, 436, 437]
* Which of the following are benefits of the microservice architecture? (e.g., Maintainability, independent deployability/scalability, fault isolation, easier adoption of new technologies) [cite: 99, 100, 101, 181, 433, 434, 435]
* Consider the difference between communication mechanisms adopted by SOA and by microservices. What of the following assertion is correct? (e.g., Microservices using lightweight/open-source vs. SOA potentially using heavyweight/proprietary mechanisms) [cite: 182, 183, 184, 185, 275, 276]
* Suppose you want to add a new record in a database using a REST web service. Are HTTP PUT and GET requests (e.g., `PUT /users` with XML body vs. `GET /adduser?name=Robert`) equivalent for this purpose? [cite: 188, 189]
* What represents a single centralized executable business process that coordinates the interaction among different services? (Service orchestration) [cite: 190, 191]
* Why should services in a microservice architecture be loosely coupled? (e.g., To minimize dependencies) [cite: 200, 201, 462]
* How data are typically handled in a microservice architecture? (e.g., Each service has its own database and data model vs. shared global data model) [cite: 202, 203, 204]
* Who is responsible for Security in the cloud, the provider or the customer, or is it shared? [cite: 261, 262] (Note: `CLC_Oral_Exam_Prep_Ans.pdf` already discusses responsibilities in source [517], this is a common phrasing).
* Consider the Service Oriented Architecture implementation of inter-process communication. Give a description of the main components in the SOA model and of the interaction flow between two nodes communicating using the SOA model. [cite: 331, 332]
* What is a capacity planning match strategy? (e.g., adding capacity in small increments as demand increases) [cite: 448]
* Assume you are using a service offered via a web application and you do not pay for the use of the service. Which feature should the service have to be classified as a cloud service? (e.g., The service should provide the illusion of having infinite resources) [cite: 449, 450]
* What is the difference between Horizontal duplication and Functional decomposition in microservices? [cite: 458, 459, 460, 461]
* Let us consider an independent components event-based system. What should a subscriber component provide? (e.g., A callback function executed when an event is activated) [cite: 463]

## Lesson 2: Containerization

* What is the difference between hypervisor of type I and hypervisor of type II? (e.g., Type I runs on bare metal, Type II runs on host OS) [cite: 11, 12, 102, 103, 214, 215, 367, 368]
* Consider the Kubernetes architecture. If you configure container autoscaling mechanisms, what component might be considered a core part of this process that monitors resource usage? (A resource usage monitor module) [cite: 17, 18, 128, 129, 130, 237, 238, 292, 293, 372, 373]
* Consider the Kubernetes architecture. What is the component that takes scaling decisions? (kube-controller-manager) [cite: 19, 20, 127, 294, 374, 375]
* Consider the Kubernetes architecture. What is the component that decides on which Kubernetes node to start a new container? (kube-scheduler) [cite: 131, 132, 296, 297, 376, 377]
* Suppose you must store a container's data on a remote host or a cloud provider rather than locally. Which Docker storage option is the most appropriate (e.g., Volumes, Bind Mount, Tmpfs mount)? [cite: 219]
* Consider the docker CLI command `docker run -dp 3000:3000 getting-started`. Which actions are performed by this command? (e.g., run in detached mode, port mapping) [cite: 223, 224, 225, 465, 466]
* Consider the docker CLI command `docker build -t getting-started .`. What actions does this command perform? (e.g., build an image from a Dockerfile in the current directory and tag it) [cite: 485, 486]

## Lesson 3: Autonomic Computing

* The Ansible automation tools is defined to be an "agentless tool that runs in a push model". What does a "push model" mean in this context? (e.g., management tool passes instructions to existing remote management frameworks) [cite: 192, 451]

## Lesson 4: Cloud Mechanisms

* Which type of scalability is facilitated by microservices? (e.g., Scaling of single functions) [cite: 181, 356]
* Assume that you are setting up a scalable web server with AWS, and for your autoscaling group, you have configured a step scaling policy. You have decided to set an alarm to start the scale-out with a threshold on the incoming traffic. When does the alarm occur? (e.g., if the average value over all instances exceeds the threshold) [cite: 24, 25, 26, 27, 28, 57, 58, 59, 60, 61, 62, 63, 121, 122, 123, 124, 125, 126, 227, 228, 229, 230, 231, 300, 301, 302, 303, 304, 358, 359, 360, 361, 362, 363, 364]
* Assume you set up a multi-threshold scale-out policy (e.g., based on CPU utilization). Why is it important to wait a certain time interval (cooldown/warm-up) before starting another scaling action after one has occurred? (e.g., to avoid ping-pong effects, allow instances to warm up) [cite: 65, 66, 67, 119, 120, 243, 244, 298, 342, 343, 344, 345, 346]
* An autoscaling algorithm is considered proactive when the scaling decision is taken on the basis of a predicted value of the scaling metrics (e.g., predicted CPU utilization in the next 5 minutes). True or False? [cite: 239, 240]
* Each ______ (e.g., Region, Availability Zone, Datacenter) is a fully isolated partition of the AWS infrastructure. Which term fits best? (Availability Zone) [cite: 254, 255]
* Consider setting up an Elastic Load Balancer to support traffic to a containerized application. Which type of ELB (e.g., Classic, Network, Application) will you typically select? (Application Load Balancer) [cite: 256, 257, 258]
* When will the public IPv4 address and external DNS hostname of an AWS EC2 instance change? (e.g., when launched, rebooted, or stopped and started again) [cite: 259, 260]
* An essential characteristic of cloud computing is measured service. How does AWS implement this feature (e.g., specific service like CloudWatch)? [cite: 457]

## Lesson 5: Cloud Storage

* Consider the BigTable datastore architecture. What are some duties of the Tablet server? (e.g., manage tablets, handle read/write requests to loaded tablets, split large tablets) [cite: 30, 31, 32, 33, 34, 35, 151, 152, 153, 154, 155, 156, 157, 308, 309, 310, 311, 312, 313, 380, 381, 382, 383, 384, 385]
* Consider the architecture of HDFS. What are some actions performed by the NameNode? (e.g., manages File System Namespace, controls file access, stores metadata, checks DataNode liveness) [cite: 36, 37, 38, 39, 138, 139, 140, 141, 142, 246, 247, 248, 249, 250, 314, 315, 316, 317, 386, 387, 388, 389, 390, 391, 474, 475, 476, 477]
* Consider the Google File System architecture. What components (or component) are responsible to handle file access? (e.g., Application, master, chunk server) [cite: 41, 42, 321, 322, 392, 393]
* Researchers and engineers at Google designed the Google File System taking into account the file access model. What is a most frequent type of file access? (Sequential read / Append write - note different answers in source files) [cite: 43, 71, 306, 394, 395]
* What is an RDD transformation in Spark? (Provide an example like `map` or `filter`, distinct from actions) [cite: 44, 323, 396, 397]
* In a MapReduce computation, if the master node fails, what action should be taken to preserve the result of the computation? (e.g., restart entire computation) [cite: 46, 47, 48, 327, 328, 329, 401, 402, 403]
* Why does the Google File System use large chunks? (e.g., optimize for large files, reduce metadata) [cite: 68, 69, 149, 150, 338, 339]
* Which is the key operation performed by the MapReduce framework (like Hadoop) that allows the Reduce task(s) to be properly executed after the completion of Map tasks? (Shuffle and sort) [cite: 72, 133, 340]
* What is the key idea behind MapReduce? (e.g., Divide and conquer algorithm design paradigm) [cite: 73, 341]
* What is an RDD action in Spark? (Provide an example like `collect` or `reduce`, distinct from transformations) [cite: 134, 398]
* In a MapReduce computation, if a map worker node fails, what action should be taken to preserve the result of the computation? (e.g., re-schedule and re-execute tasks assigned to the failed worker) [cite: 135, 136, 137, 404, 405, 406]
* Consider the Google File System architecture. What component(s) are responsible for creating a file? (The master) [cite: 143, 144, 427, 428]
* A Bigtable is a sparse, distributed, persistent multidimensional sorted map. Which combination of keys (e.g., Row key and column key; Row key, column key and timestamp; Row key, family key and timestamp) does *not* uniquely locate a specific cell version or does not allow to locate a cell? [cite: 145, 146, 251, 252, 253, 318, 319, 320, 416, 417, 418, 442, 443, 467, 468, 484]
* Bigtable uses timestamps to maintain versioning of the data stored. What is a mechanism to garbage collect cell versions automatically? (e.g., Keep only new-enough versions, or last N versions) [cite: 147, 148, 444, 445, 446, 469, 470]
* Considering the write protocol of HDFS, who is in charge of sending the "write successful" acknowledgement back to the client (or to the NameNode, depending on specific interpretations of "successful")? (The client often receives the final success ack after the pipeline confirms) [cite: 289, 290, 414, 415]
* Hadoop MapReduce optimizes data locality. If data-local execution isn't possible, what alternative deployment policies are used? (e.g., Rack local deployment) [cite: 325, 326, 399, 400]
* Provide and comment on an example of a MapReduce program. [cite: 330]
* Explain the difference between a transformation of an RDD and an action on an RDD in Spark (bring an example). [cite: 337]
* Researchers and engineers at Google designed the Google File System taking into account file characteristics. What is the typical range for file size? (e.g., Kilobytes to Gigabytes, Megabytes to Terabytes) [cite: 471, 472]
* What is the chunk size in the Google File System? (e.g., 64MB) [cite: 696, 473]
* Consider the BigTable datastore architecture. What are some responsibilities of the Master node? (e.g., assign tablets, detect server addition/expiration, balance load, garbage collection of files in GFS) [cite: 478, 479, 480, 481, 482, 483]