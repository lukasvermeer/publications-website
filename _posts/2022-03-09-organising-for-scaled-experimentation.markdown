---
layout: post
title: "Organising for scaled experimentation"
date: 2022-03-09
tags: ["experimentation", "mission", "vision", "organisation"]
original:
  source_name: Vista DnA Blog
  source_url: https://vista.io/blog/organising-for-scaled-experimentation
---

*Written by Lukas Vermeer and Flavio De Souza.*

We want to build a strong culture of experimentation at Vista, so that we can confirm that we are delivering—and continue to deliver—jaw-dropping customer value to our customers. With the [introduction of the Experimentation Hub]( {{site.baseurl}}{% post_url 2022-03-09-organising-for-scaled-experimentation %}) ), we took the first steps towards that vision.

The Experimentation Hub is central to our strategy, but it alone is not sufficient to scale experimentation across our organisation. In this second post in our series, we will expand our scope and describe the hybrid approach we are taking to foster and support a culture of experimentation.

# Scaling embedded experimentation

Effective execution of an experiment requires a diverse set of skills and roles. It would be unreasonable to expect any individual or team to have all the skills and roles required. Instead, we aim to ensure that teams have easy access to the required skillsets and tools by clearly defining their boundaries and supporting escalation paths. We strive for *Freedom Within a Framework* (FWF): empowering teams to operate autonomously (e.g., without decision-making or approval loops) in service to their objectives.

We distinguish between tasks and skills related to the execution of individual experiments and tasks and skills related to the tools and processes that support experimentation in aggregate. The former will be as decentralised and autonomous as possible, while the latter will be mostly centralised. This separation allows central teams to focus on providing platforms; systems which provide clear guardrails, best practices, methodologies, and configurable cross-Vista systems which set relevant frameworks for all teams.

At a high level, this means that we define three different types of experimentation-related skillsets which are housed in different organisational focal areas.

1. Skills related to **the domain being experimented in** should—as much as possible—be decentralised and housed in the cross-functional team that is running the experiment. This includes domain-specific skills and knowledge regarding how experiments are prioritised, designed, coordinated, executed, and evaluated; for example which content management systems are used to set up experiments on a specific website, or which metrics are most relevant for an experiment in a customer care centre and how to interpret them. This will require the allocation of local experts within the domain (see [this paper](https://exp-platform.com/Documents/2019-FirstPracticalOnlineControlledExperimentsSummit_SIGKDDExplorations.pdf) section 7.2), which we will call *Experimentation Ambassadors*.
2. Skills related to **the experimentation process and analysis frameworks**, in general, will be housed centrally in the *Experimentation Centre of Excellence*. This includes collecting, documenting, evangelising, and teaching best practices, as well as building necessary data products. This Center of Excellence will also organise and facilitate the Ambassador program, as well as provide second-line support to Ambassadors through *Experimentation Consultants*. The Centre of Excellence will collaborate closely with the Experimentation Product Development team to identify and address points of friction in the experimentation frameworks and tools to reduce the cost of scaling and adoption of experimentation.
3. Skills related to **the development and deployment of technology and tools** used to manage and execute experiments and collate results will be housed centrally in a team dedicated to *Experimentation Product Development*. This includes the development of software libraries that can be used by other development teams to integrate into the experimentation platform, development and maintenance of the data processing pipelines for experimentation data, and development of the internal user-facing platforms and tools used to manage, execute, review and document experiments. This Experimentation Product Development team will also provide technical support in cases where the Experimentation Centre of Excellence lacks the technical depth of knowledge required.

The Experimentation Center of Excellence and Experimentation Product Development team (together known as the Experimentation Hub) are tightly linked and centralised. This cross-functional FWF-platform team is overseen by the Director of Experimentation. They share the same North Star, key performance indicators, and overarching objectives to create decentralised freedom to experiment within a shared framework. Specific domains and Experimentation Ambassadors operate within that framework and may also choose to adopt these key performance indicators—tailored to their specific domain—if they see fit.

![An organisational diagram showing the "Experimentation Hub" and relations to other teams.]({{site.baseurl}}{% link assets/2022-03-09-diagram.jpeg %})

# The Experimentation Ambassadors

Knowing everything about statistics is not sufficient to provide good experimentation support. One also needs to understand the business context, objectives, and relevant technology and metrics that exist within a domain. Running an experiment on email marketing campaigns requires knowledge and skills that are not needed when running experiments on a call routing system in a customer support centre, and vice versa. Although there is overlap, each domain has its own specifics that need to be understood to be able to support experiment execution.

Experimentation Ambassadors are the first line of support for any experiment in a domain. A purely centralised experimentation execution and support function would require deep knowledge of all domains in one place. This is often not feasible and does not scale well. Therefore, having a decentralised Experimentation Ambassador role—in addition to a centralised support function—with domain-specific knowledge is necessary to scale experimentation and provide better support for each domain.

Some of the Experimentation Ambassadors responsibilities include:

- acting as a facilitator giving end-to-end guidance and support for experiments in their domain;
supporting the testing agenda and test coordination within their domain;
- encouraging best practices;
- supporting the education of experimenters around them;
- facilitating peer-review;
- granting domain users access to experimentation tools;
- measuring and reporting on domain testing program to their leadership.

To enable teams to truly operate autonomously, domains are responsible for the budget required to assign or recruit Experimentation Ambassadors within their own area. In some cases, depending on needs, this Experimentation Ambassador role might be filled by an existing embedded Analyst, in others, the domain might choose to recruit their own dedicated Ambassador(s).

To facilitate the recruitment of Experimentation Ambassadors, the Experimentation Hub provides interview scripts, technical challenges, and support for hiring managers who wish to recruit an Experimentation Ambassador for their domain as part of the Experimentation Ambassador Program. This is FWF applied at the staffing level: the central team defines the framework within which teams are free to hire as much support as they think they will need.

# The Experimentation Ambassador Program

In order to be able to provide experimentation support, Experimentation Ambassadors need to have access to and learn to master fundamental experimentation knowledge. Growing generic experimentation knowledge is easier to scale, because we can provide the same experimentation education and second-line support to all Experimentation Ambassadors, even if they are in wildly different business domains.

The Experimentation Hub is centrally responsible for the Experimentation Ambassador Program framework. The program focuses on the recruitment, education, and shared ceremonies for Experimentation Ambassadors throughout the organisation.

The training program is structured into 3 sub-programs and includes live sessions, self-paced material covering the modules, and discussions based on case studies. 

1. The **onboarding program** seeks to empower Ambassadors with the necessary knowledge and essential tools to support domain teams in carrying out experiments as autonomously as possible.
2. The **ongoing program** emphasises deep learning on the experimentation curriculum, toolkit, communication, and best practices, preparing the Ambassadors to become successful experimenters and multipliers.
3. The **experimentation ceremonies** are sessions where Ambassadors will also be able to continue developing their experimentation expertise.  These ceremonies are designed to foster collaboration between Ambassadors and the Experimentation Hub and grow the Vista experimentation program efficiently.

The program also offers a “mentorship service”, where members of the Centre of Excellence will be available to guide ambassadors on their own domain projects.

One of the onboarding sessions over Zoom with several Experimentation Ambassadors and the Experimentation Centre of Excellence.

![One of the onboarding sessions over Zoom with several Experimentation Ambassadors and the Experimentation Centre of Excellence.]({{site.baseurl}}{% link assets/2022-03-09-ambassadors-zoom.png %})

# Piloting the Experimentation Ambassadors

With the introduction of the Experimentation Ambassadors Program, we are taking the next step on our journey towards a strong culture of experimentation. We are taking a hybrid approach to our organisational model for experimentation, because some tasks can benefit from centralisation in a cross-functional FWF-platform team, while others can enable scale through decentralised Freedom Within a Framework. The key to success will be to ensure that our experimentation framework is transparent and flexible, so that centralisation does not result in bottlenecks or friction, and decentralisation does not result in chaos or lack of visibility.

The Experimentation Ambassadors Program itself is also a kind of experiment. We are constantly collecting feedback and iterating to improve. We are aiming for a sweet spot between centralisation and decentralisation, where our hub and spokes together make our flywheel spin faster and faster. Growing our experimentation maturity, one experiment at a time.
