---
layout: post
title:  "Learn J2EE Architecture"
date:   2016-07-17 19:14:40 +0800
categories: jekyll update
tags:	[J2EE,Architecture,reading]
---

Topics
===========================================================
❑ Distributed and non-distributed applications, and how to choose which model is appropriate
❑ The implications for J2EE design of changes in the EJB 2.0 specification and the emergence of web services
❑ When to use EJB
❑ Data access strategies for J2EE applications
❑ Four J2EE architectures, and how to choose between them
❑ Web tier design
❑ Portability issues

Note: 
	In particular,the message I'ii try to get across will be that we should apply J2EE to realize OO desgin,not let J2EE technologies dicate object desgin.

CH01T1:Goals of an Enterprise Architecture
===========================================================
A well-designed Application should meet the following goals:
1) Be robust
      reliable
	  bugfree
2) Performant and scalable
	  increased load
	  clustering
	  sophisticated
3) Take advantage of OO design principles
      benefits for complex systems
      use of proven design patterns
4) Avoid unnecessary complexity
	  XP
	  blance the requirements and solutions's cpmplexity
5) Be maintainable and extensible
	  the most expensive phase of the software lifecycle
	  avoid tightly-coupled components
6) Be delivered on time
	  Productivity is a vital consideration,which is too often neglected when approaching J2EE.
7) Be easy to test
	  Essential activity throughout the software lifecycle	  
8) Promote reuse
	  Enterprise Software(ES) must fit into an organization's long term strategy.Thus it's important to foster reuse,so that code duplication is minimized(within and across projects) and investment leveraged to the full.Code reuse usually results from good OO design practice,while we should also consistently use valuable infrastructure provided by the application server where it simplifies application code.

Note:Depending on an application's business requirements,we may also need to meet the following goals:
9) Support for multiple client types
	  Web Applications
	  Standalone Java GUIs(using Swing or other windowing systems)
      Java applets
My Trend	  Html5: Can you kill the app?
However, such support is often unnecessary, as "thin" web interfaces are being more and more widely used, even for applications intended for use within an organization (ease of deployment is one of the major reasons for this).

10)Portability
	How important is portability between resources, such as databases used by a J2EE application? How important is portability between application servers? Portability is not an automatic goal of J2EE applications. It's a business requirement of some applications, which J2EE helps us to achieve.

Appendix
======================================================================
1)The importance of the last two goals is a matter of business requirements, not a J2EE article of faith. We can draw a dividend of simplicity that will boost quality and reduce cost throughout a project lifecycle if we strive to achieve only goals that are relevant.

2)The concept of design patterns was popularized in OO software development by the classic book Design Patterns: Elements of Reusable Object-Oriented Software from Addison Wesley, (ISBN 0-201-63361-2), which describes 23 design patterns with wide applicability. These patterns are not technology-specific or language-specific.

CH01T2: Deciding whether to use a distributed architecture
======================================================================
1)J2EE provides outstanding support for implementing distributed architectures
2)based on the use of EJBs whth remote interfaces,which enable the application server to conceal much of the complexity of access to and management of distributed components

In my experience, the deployment flexibility benefits of distributed applications are exaggerated. Distribution is not the only way to achieve scalable, robust applications.Most J2EE architectures using remote interfaces tend to be deployed with all components on the same servers, to avoid the performance overhead of true remote calling. This means that the complexity of a distributed application is unnecessary,since it results in no real benefit.

CH01T3:New Considerations in J2EE Design
======================================================================
J2EE 1.2 Specification offered simple choice:
	1)EJBs had remote interfaces and could be used only in distributed applications
    2)Remote Method Invocation(RMI)(Over JRMP or IIOP) was the only choice for supporting remote clients

1)The Ejb 2.0 Specification allows EJBs to hava local interfaces,in addition to or instead of,remote interfaces.EJBs can be invoked through their local interfaces by components in an integrated J2EE application running in same JVM:for example,components of a web application.
2)The emergence of the XML-based Simple Object Access Protocol(SOAP) as a widely accepted,platform-agnostic standard for RMI,and widespread support for web services.

CH01T4:When to use EJB
======================================================================
1) Goal of the EJB Specificaton:
	a) simplify application code
	b) application developers will not have to understand low-level transaction and state management details,multi-threading,connection pooling,and other complex low-level APIs.

2) In Actions Shows that,Using EJB makes applications:
	a) harder to test,as they are heavily dependent on container services.
    b) harder to deploy,
	    complex classloader issues
		complex deployment descriptors
		Slower development-deployment-test cycles
Note:Most practical frustrations with J2EE relate to EJB. This is no trivial concern; it costs time and money if EJB doesn't deliver compensating benefits.
	c) Using EJB with remote interfaces may hamper practicing OO design

The pernicious effects of unnecessarily using EJBs with remote interfaces include:
	Interface granularity and method signatures dictated by the desire to minimize the number of remote method calls. If business objects are naturally fine-grained (as is often the case), this results in unnatural design.
	The need for serialization determining the design of objects that will be communicated over RMI. For example, we must decide how much data should be returned with each serializable object – should we traverse associations and, if so, to what depth? We are also forced to write additional code to extract data needed by remote clients from any objects that are not serializable.
	A discontinuity in an application's business objects at the point of remote invocation.

These objections don't apply when we genuinely need distributed semantics. In this case, EJB isn't the cause of the problem but an excellent infrastructure for distributed applications. But if we don't need distributed semantics, using EJB has a purely harmful effect if it makes an application distributed. As we've discussed, distributed applications are much more complex than applications that run on a single server. EJB also adds some additional problems we may wish to avoid:
	1) Using EJB may make simple things hard
	2) Reduced choice of application servers

Questionable Arguments for Using EJB
	1) To ensure clean architecture by exposing business objects as EJBs
	2) To permit the use of entity beans for data access
	3) To develop scalable,robust applications
Compelling Arguments for Using EJB
	1) To allow remote access to application components
	2) To allow application components to be spread across multiple physical servers
	3) To support multiple Java or CORBA client types
	4) To implement message consumers when an asynchronous model is appropriate

Arguments for Using EJB to consider on a Case-by-Case Basis
	1) To free application developers from writting complex multi-threaded code.
		EJB's simplification of multi-threaded code is a strong,but not decisive,argument for using EJB.
	2) To use the EJB container's transparent transaction management
		EJB's Container-Managed Transactions(CMT)
		will still hava choice: Java Transaction API(JTA)

Note that the J2EE transaction management infrastructure (for example, the ability to coordinate transactions across different enterprise resources) is available to all code running within a J2EE server, not merely EJBs; the issue is merely the API we use to control it.
	
	The availability of declarative transaction management via CMT is the most compelling reason for using EJB.

	3) To use EJB declarative support for role-based security
		J2EE offers both programmatic and declarative security.
		If we don't use EJB,only the programmatic approach is available.

	4) The EJB infrastructure is familiar
	If the alternative to using EJB is to develop a substantial subset of EJB's capabilities, use of EJB is preferable even if our own solution appears simpler. For example, any competent J2EE developer will be familiar with the EJB approach to multi-threading, but not with a complex homegrown approach, meaning that maintenance costs will probably be higher. It's also better strategy to let J2EE server vendors maintain complex infrastructure code than to maintain it in-house.

Attention
=========
EJBs are a good solution to problems of distributed applications and complex
transaction management. However, many applications don't encounter these
problems. EJBs add unnecessary complexity in such applications. An EJB solution can be likened to a truck and a web application to a car. When we need to perform certain tasks, such as moving large objects, a truck will be far more effective than a car, but when a truck and a car can do the same job, the car will be faster, cheaper to run,more maneuverable and more fun to drive.

CH01T5: Accessing Data
======================================================================
	EJB:	Entity beans
	data source connection pooling(container support)
    crucail design issue

J2EE Data Access Shibboleths
	challenged:
	 1) Portability between databases is always essential
	 2) Object/Relational(O/R) Mapping is always the best solution when working with relational database.

Database portability isn't free,and may lead to unnecessary complexity and the sacrifice of performance.

often a "domain object model" must be shoehorned onto a relational database,with no concern for efficiency.

Whatever the data access strategy we use,it's desirable to decouple business logic from the details of data access,through an abstraction layer.

	It's often assumed that entity beans are the only way to achieve such a clean separation between data access and business logic. This is a fallacy. Data access is no different from any other part of a system where we may wish to retain a different option of implementation. We can decouple data access details from the rest of our application simply by following the good OO design principle of programming to interfaces rather than classes. This approach is more flexible than using entity beans since we are committed only to a Java interface (which can be implemented using any technology), not one implementation technology.

Entity Beans
	don't tie us to a particular type of database,but do tie us to the EJB container and to a particular O/R mapping technology.

There exist serious doubts regarding the theoretical basis and practical value of entity beans. Tying data access to the EJB container limits architectural flexibility and makes applications hard to test. We are left with no choice regarding the other advantages and disadvantages of EJB. Once the idea of entity
beans having remote interfaces is abandoned (as it effectively is in EJB 2.0), there's little justification for modeling data objects as EJBs at all.

Despite enhancements in EJB 2.0, entity beans are still under-specified. This makes it difficult to use them for solving many common problems (entity beans are a very basic O/R mapping standard). They often lead to inefficient use of relational databases, resulting in poor performance.

Java Data Objects(JDO)
======================
	JDO is a recent specification developed under the Java Community Process that describes a mechanism for the persistence of Java objects to any form of storage. JDO is most often used as an O/R mapping, but it is not tied to RDBMSs. For example, JDO may become the standard API for Java access to
ODBMSs. JDO offers a more lightweight model than entity beans. Most ordinary Java objects can be persisted as long as their persistent state is held in their instance data. Unlike entity beans, objects persisted using JDO do not need to implement any special interfaces. JDO also defines a query language
for running queries against persistent data. It allows for a range of caching approaches, leaving the choice to the JDO vendor.
	JDO is not currently part of J2EE. However, it seems likely that it will eventually become a required API in the same way as JDBC and JNDI.
	
	JDO provides the major positives of entity beans while eliminating most of the negatives. It integrates well with J2EE server transaction management, but is not tied to EJB or even J2EE. The disadvantages are that JDO implementations are still relatively immature, and that as a JDO implementation doesn't come with most J2EE application servers, we need to obtain one from (and commit to a relationship with) a third-party vendor.	

Other O/R Mapping Solutions
=============================
	Leading O/R mapping products such as TopLink and CocoBase are more mature than JDO. These can be used anywhere in a J2EE application and offer sophisticated, high-performance O/R mapping, at the price of dependence on a third-party vendor and licensing cost comparable to J2EE application servers.
These solutions are likely to be very effective where there is a natural O/R mapping.

JDBC
=============================
	Implicit J2EE orthodoxy holds that JDBC and SQL (if not RDBMSs themselves) are evil, and that J2EE should have as little to do with them as possible. I believe that this is misguided. RDBMSs are here to stay, and this is not such a bad thing.
	The JDBC API is low-level and cumbersome to work with. However, slightly higher-level libraries (such as the ones we'll use for this book's sample application) make it far less painful to work with. JDBC is best used when there is no natural O/R mapping, or when we need to use advanced RDBMS features
like stored procedures. Used appropriately, JDBC offers excellent performance. JDBC isn't appropriate when data can naturally be cached in an O/R mapping layer.

State Management
============================
	Another crucial decision for J2EE architects is how to maintain server-side state. This will determine how an application behaves in a cluster of servers (clustering is the key to scalability) and what J2EE component types we should use.
	It's important to decide whether or not an application requires server-side state. Maintaining server-side state isn't a problem when an application runs on a single server, but when an application must scale by running in a cluster, server-side state must be replicated between servers in the cluster to allow failover and to avoid the problem of server affinity (in which a client becomes tied to a particular server). Good application servers provide sophisticated replication services, but this inevitably affects performance and scalability.

Attention
=========
1) If we do require server-side state, we should minimize the amount we hold.
2) Applications that do not maintain server-side state are more scalable than
applications that do, and simpler to deploy in a clustered environment.

	If an application needs to maintain server-side state, we need to choose where to keep it. This depends partly on the kind of state that must be held: user interface state (such as the state of a user session in a web application), business object state, or both. Distributed EJB solutions produce maximum scalability
with stateless session beans, regardless of the state held in the web tier.
	J2EE provides two standard options for state management in web applications: HTTP session objects managed by the web container; and stateful session EJBs. Standalone clients must rely on stateful session beans if they need central state management, which is another reason why they are best supported by EJB architectures. Surprisingly, stateful session EJBs are not necessarily the more robust of the two options (we discuss this in Chapter 10) and the need for state management does not necessarily indicate the use of EJB.



======================================================================
======================================================================
======================================================================
======================================================================

>
Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
