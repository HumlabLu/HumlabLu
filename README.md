


# The LANG-TRACK-APP

#### Table of Contents
1. Overview (this document)
2. [Web app, server and database](#example2)
3. [Mobile apps and Firebase](#LTAMobileApps.md)

### Introduction

The LANG-TRACK-APP (LTA) was developed in the context of a research project on [Second Language Acquisition](https://en.wikipedia.org/wiki/Second-language_acquisition) (SLA). SLA is the field of research studying the process of learning additional languages in different settings across the age span.

Some research in the field of SLA targets learners’ interactions with different languages in everyday life and the impact of these interactions on language learning. However, studying exposure to and use of languages in everyday life is a methodological challenge, a challenge that the LTA was specifically designed to address.

The LTA is a native smartphone application running on iOS and on Android platforms released under license NN, which is linked to a back-end system. Today, the use of smartphones is widespread and people tend to carry them with constantly which means that researchers can sample data frequently from participants (in our case language learners) at different moments and in different settings and places in their everyday activities. The combination of the LTA with [Experience Sampling Methods](https://en.wikipedia.org/wiki/Experience_sampling_method) enables us to gain a richer understanding of how learners meet and use different languages (cf. Arndt et al., 2021 for discussion).

### Why  develop the LTA?

Principal Investigators (PI) of research projects need to balance the responsibility to comply with international, national and/or local conditions for data stewardship and data security, the requirements of Open Science, and the flexibility to design research tools to address specific research questions. There are therefore three main reasons as to why the LTA was developed.

1.  The design of the LTA and the setup of the client-server configuration allow researchers to have extensive control of the dataflow and data processing with minimal involvement of third party providers (including cloud services). The EU [General Data Protection Regulation](https://eur-lex.europa.eu/legal-content/EN/TXT/?qid=1528874672298&uri=CELEX:02016R0679-20160504) (GDPR) requires that data processing is lawful, fair and transparent. According to the GDPR, controllers and processors of data must, under certain circumstances, provide safeguards if data is transferred to a third country or an international organization. Additional national and/or local requirements on data processing and data transfer may also exist. Therefore, the LTA’s design and setup aim to minimize the need of safeguards with respect to reliance on third party providers. The LTA communicates and transfers data to a local server that can be set up in the research environment in accordance with local infrastructure and data safety regulations (see below for details).
    

2.  Sharing the full code for the LTA (including the backend services) with the research community will enable use and adaptations of the tools in the interest of Open Science. Therefore, new iterations of the LTA must be shared by research groups with the aim of securing the long-term sustainability of the tools.
    

3.  By developing the LTA rather than relying on a third party provider of similar services, we also allowed for customised adaptations of the application, data flow and data management specific to the overall project aims and needs. By allowing researchers to control there can be greater flexibility and more time-aligned adaptations than what is otherwise possible.
