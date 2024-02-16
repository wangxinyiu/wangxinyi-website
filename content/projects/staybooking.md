---
title: StayBooking - An online stay rental application
type: page
---
- [What I did](#what-i-did)
- [Backend Structure](#backend-structure)
- [Data Model](#data-model)
- [Compelete Code](#compelete-code)
- [Challenge](#challenge)
  - [Elastic Search(Star)](#elastic-searchstar)
    - [elastic search 实现步骤简述](#elastic-search-实现步骤简述)
    - [用Google cloud storage（GCS）存储照片等流媒体](#用google-cloud-storagegcs存储照片等流媒体)
    - [为什么elastic search的全局搜索能力这么强呢？](#为什么elastic-search的全局搜索能力这么强呢)
- [Potential areas of improvement](#potential-areas-of-improvement)
## What I did
**Deomo: https://recordit.co/cXdWcj0ahV**
- Designed and built a single-page web application using __React__. Bootstrap the development with mature component library __Ant Design__. 
- Implemented the backend services based on __Spring Boot__ to support stay upload, delete, search, and reserve functionality. 
- Used __MySQL__ to store user-generated data, e.g. stay information and reservation history, and utilized __Google Cloud Storage__ to store media files for uploaded stays. 
- Created __geo index__ by __Elasticsearch__ to support geo-based stay search based on user’s selected locations. 
- Implemented __token-based__ server-side user authentication based on the __Spring Security framework__. 
- Deployed the backend service to __Google App Engine__ for better scalability and reliability.

## Backend Structure
![backend_structure](/images/staybooking1.jpg)

## Data Model
![backend_structure](/images/staybooking2.png)

## Compelete Code
https://github.com/wangxinyiu/staybooking

## Challenge
### Elastic Search(Star)
<!--
StayBooking是一个类似于Aribnb的project，让用户能够上传自己的住宿信息，并且允许其他用户根据地理位置搜索附近的住宿。
我想做一个project 这个project是类似airbnb的project，用户可以上传自己的host，同时还可以根据地理位置search附近的host。
我现在的难点是不知道该用什么database去存储这些信息。
database的选择，因为每个host有照片，地理位置，名字等等的field，

但是根据用户的位置来全局搜索附近的host对于sql来说是一个复杂的事情。在大量访问的情况下，查询和管理起来非常复杂。

通过Elasticsearch创建了地理索引，根据用户选择的位置进行基于地理位置的住宿搜索。

所以我了解到了elastic search
因为 elastic search的 基于地理位置的数据索引和查询和。。。功能 功能，这对于基于位置的搜索和分析非常有用，且elastic search处理地理空间和全文搜索需求


Elasticsearch 支持基于地理位置的数据索引和查询，可以讲坐标自动转换到坐标系中，可以提供高性能全文搜索和地理位置搜索。
-->

**Situation（情境）**：开发一个类似于Airbnb的在线住宿分享和搜索平台。(Develop an Airbnb-like online accommodation sharing and search platform.)   

**Task（任务）**：需要选择合适的数据库来存储包括照片、地理位置、住宿名称在内的信息，并实现基于地理位置的住宿搜索功能。(There is a need to select a suitable database to store information including photos, geographic locations, accommodation names, and to implement a location-based accommodation search function.)          

**Action（行动）**：选择使用Google Cloud Storage来存储照片等媒体文件，利用Elasticsearch来创建地理索引并实现基于地理位置的高效搜索。(Choose to use Google Cloud Storage to store media files such as photos, and Elasticsearch to create geo-indexing and enable efficient location-based search.)

> 在选择技术栈的过程中，特别是面对存储媒体文件和实现基于地理位置的搜索功能的需求时，我进行了以下考虑和分析：   

> **对媒体文件存储的考虑**：(streaming media)  
> 1. 容量和扩展性：考虑到用户上传的照片和视频可能会占用大量的存储空间，并且随着平台用户数量的增加，这些需求只会增加，我需要一个能够轻松扩展的存储解决方案。        
> 2. 数据访问速度和全球可达性：为了提供良好的用户体验，确保全球用户都能快速访问这些媒体文件是非常重要的。      
> 3. 成本效率：存储成本是另一个重要考量，特别是对于刚起步的项目，找到一个成本效率高的解决方案至关重要。 

> **对地理位置搜索的考虑**：        
> 1. 复杂查询的支持：除了地理位置外，用户可能还会基于价格、设施等其他条件进行搜索，所以需要一个能够支持复杂查询的系统。        
> 2. 实时性：新上线的住宿信息需要能够尽快被索引并加入搜索结果中，以提供最新的数据给用户。

**Result（结果）**：通过结合Google Cloud Storage和Elasticsearch，项目能够有效地存储用户上传的信息并支持快速的地理位置搜索，满足用户对于查找附近住宿的需求。(By combining Google Cloud Storage and Elasticsearch, the project is able to efficiently store user uploads and support fast geo-location searches to fulfill users' needs for finding nearby accommodations.)         

#### elastic search 实现步骤简述
使用MySQL存储主要的业务数据，利用其成熟的事务处理和复杂查询能力；同时，使用Elasticsearch来处理地理位置搜索和全文搜索需求，利用其快速的搜索能力和对地理空间数据的良好支持。

1. **数据存储**：将用户信息、住宿信息等业务数据存储在MySQL数据库中。这些信息包括用户的注册信息、住宿的描述、价格、设施信息等。

2. **同步数据到Elasticsearch**：选择需要支持快速搜索的数据（如住宿信息）同步到Elasticsearch。这可以通过定期的批处理作业完成，或者使用像Logstash这样的数据管道工具实时同步。确保每条住宿记录中包括地理位置信息（例如，经纬度）。

3. **创建地理空间索引**：在Elasticsearch中针对住宿信息创建地理空间索引。使用Elasticsearch的地理空间特性（如geo_point类型字段）来存储和索引住宿的地理位置。

4. **基于地理位置的搜索**：当用户通过界面选择一个位置进行住宿搜索时，后端服务将构造一个Elasticsearch查询，该查询基于用户指定的地理位置和其他可能的筛选条件（如价格范围、设施要求等）进行搜索。Elasticsearch会根据地理索引快速返回相关的住宿选项。

5. **展示结果**：后端服务从Elasticsearch获取搜索结果后，可能需要再次查询MySQL数据库以获取完整的住宿信息（如果Elasticsearch中没有存储全部数据）。最后，将这些信息展示给用户。

**注意事项**

- **数据一致性**：保持MySQL和Elasticsearch之间数据的一致性是重要的。需要确保当MySQL中的数据发生变化时，相应的变化也同步到Elasticsearch。

- **性能与成本**：虽然Elasticsearch提供了强大的搜索能力，但其运行和维护也会带来额外的成本和复杂性。权衡性能需求和成本是选择使用Elasticsearch的一个重要考虑因素。

#### 用Google cloud storage（GCS）存储照片等流媒体
使用了SQL数据库与Google Cloud Storage（GCS）结合的，在SQL数据库中存储一个指向GCS中存储资源（如照片、视频等媒体文件）的链接（URL）。

**工作流程**        
1. 上传文件到GCS：当用户上传照片或其他媒体文件时，这些文件被上传到GCS的一个特定的桶（Bucket）中。每个文件上传后，GCS会提供一个唯一的URL，用于访问或引用这个文件。

2. 存储链接到SQL数据库：在SQL数据库的相应记录中，你会存储指向GCS中文件的URL。例如，如果你的数据库中有一个accommodations表格用于存储住宿信息，它可能包括name（名称）、location（地理位置）等字段，以及一个photo_url字段，后者存储的就是指向GCS中相应照片的URL。

3. 数据检索：当需要展示住宿信息（包括照片）给用户时，应用后端会从SQL数据库中查询住宿记录，获取照片的URL，并在前端页面中通过这个URL从GCS加载照片。


**为什么用GCS？**       

- 分离存储：通过将大型的媒体文件存储在GCS中，可以避免SQL数据库因管理大量大型二进制对象（BLOBs）而变得臃肿，有助于提高数据库的性能和管理效率。       
- 可扩展性和可靠性：GCS提供了高度的可扩展性和数据持久性，能够有效地存储和管理大量的媒体文件，同时还支持全球访问和高可用性。     
- 安全性和权限控制：GCS提供了细粒度的权限控制，你可以配置谁可以访问或修改存储的文件，增强了数据的安全性。       

#### 为什么elastic search的全局搜索能力这么强呢？
1. **倒排索引**：Elasticsearch使用倒排索引来实现其搜索功能。倒排索引是一种索引，其中每个唯一的单词都有一个包含该单词的所有文档列表。这种结构使得对任意词的搜索变得非常快速，因为搜索引擎可以直接定位到包含搜索词的文档，而不需要遍历所有文档。

2. **分布式架构**：Elasticsearch天生支持分布式，数据可以跨多个节点分散存储。这种架构不仅提高了系统的可用性和容错能力，还允许Elasticsearch并行处理大量数据，显著提高搜索速度和处理能力。

3. **高效的数据结构**：Elasticsearch内部使用高效的数据结构和算法，比如压缩的倒排索引、缓存机制等，进一步提升了搜索的速度和准确性。

4. **灵活的查询语言**：Elasticsearch提供了一种丰富而灵活的查询DSL（领域特定语言），支持各种类型的搜索，包括简单的文本匹配、复杂的布尔操作、过滤、排序、聚合等。这使得开发者可以构建复杂且精确的查询来满足各种搜索需求。

5. **实时搜索**：Elasticsearch几乎可以实现实时搜索。当文档被索引到Elasticsearch后，它几乎立即就可以被搜索到，这对于需要快速响应用户查询的应用场景非常重要。

6. **自然语言处理功能**：Elasticsearch支持文本分析和自然语言处理功能，如分词、同义词处理、拼写纠正等。这些功能使得Elasticsearch能够理解和处理人类语言，提供更为相关和准确的搜索结果。

7. **易于扩展和维护**：Elasticsearch的设计使其易于扩展和维护。你可以简单地通过添加节点来扩展集群，以处理更多数据和查询，而不需要对现有应用程序进行大的修改。

## Potential areas of improvement
**分布式系统的考虑**
   1. 扩展性: 随着用户数量和数据量的增长，单个服务器可能无法高效处理所有的请求。采用分布式架构可以通过添加更多的服务器来水平扩展系统，从而提高处理能力和存储容量。
   2. 可用性和容错性: 在分布式系统中，数据可以在多个节点间复制，这不仅提高了数据的可用性，也意味着即使部分节点失败，系统仍然可以继续运行，从而提高了整体的容错性。
   3. 地理分布:通过在不同地理位置部署服务器节点，可以减少数据传输的延迟，提供更快的响应速度，改善全球用户的体验。          

**多线程处理的考虑**        
1. 性能优化: 多线程可以充分利用服务器的多核CPU资源，通过并行处理提高应用程序的处理速度和响应时间。
2. 复杂度管理: 虽然多线程能提高性能，但也增加了编程的复杂度。需要仔细管理线程之间的同步和通信，避免出现死锁、竞态条件等问题。
3. 资源管理: 合理分配和管理线程可以避免资源浪费和冲突，比如通过线程池来管理线程的生命周期和数量。   

**数据转移的问题**      

将原来存储在本地MySQL数据库中的数据迁移到一个支持分布式处理的环境中。       
1. 分布式数据库解决方案
   - 云托管数据库：考虑使用云服务提供商（如Amazon RDS、Google Cloud SQL、Azure Database）提供的托管MySQL服务。这些服务通常提供高可用性、自动备份、扩展性以及分布式处理能力。将数据迁移到云托管的数据库，可以让你轻松地将数据存储在分布式环境中，而无需自己管理数据库服务器。
   - NoSQL数据库：如果你的应用可以从NoSQL数据库中受益（例如，需要更高的写入吞吐量或更灵活的数据模型），可以考虑迁移到如Cassandra、MongoDB或Amazon DynamoDB这样的分布式NoSQL数据库。这些数据库天生就是为分布式环境设计的，能够在多个数据中心之间复制数据，提供高可用性和水平扩展。
  
2. 分布式文件存储           
    对于需要存储的大型媒体文件（如照片、视频），可以使用分布式文件存储服务，如Amazon S3、Google Cloud Storage或Azure Blob Storage。这些服务提供了高度的可扩展性、数据冗余和全球访问速度，非常适合作为存储大量媒体文件的解决方案。

3.  数据迁移策略        
    - 逐步迁移：在迁移过程中，你可能需要保持原有的MySQL数据库运行一段时间，直到新的分布式环境稳定运行。逐步迁移数据，同时更新应用程序的数据访问层，确保它们指向新的数据存储。
    - 数据同步：如果可能，使用数据迁移工具或服务来同步旧数据库和新数据库之间的数据，直到完全切换到新环境。

