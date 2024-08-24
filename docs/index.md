# Final Evaluation Report for GSOC 2024

<div align="center">
<img src="" alt="drawing" width="500"/>
</div> 


## Details

|  |  |
| --- | --- |
| Name | [Tvisha Vedant](https://github.com/tvilight4) |
| Organisation | [Neurobagel](https://neurobagel.org/), [INCF](https://www.incf.org/)  |
| Mentor | [Arman Jahanpour](https://github.com/rmanaem), [Sebastian Urchs](https://github.com/surchs) ,[Alyssa Dai](https://github.com/alyssadai),[Brent Mcpherson]()  |
| Project | [LLM-assisted tool to annotate research data with machine-understandable, semantic data dictionaries](https://summerofcode.withgoogle.com/programs/2024/projects/RbOlafUP) |

### Proposal Link :- []()


## Table of Contents

- [About Project](#about-project)
- [Motivation](#motivation-behind-using-cppinterop)
- [Summary Of Work Done](#summary-of-work-done)
- [Contributions (PRs)](#contributions)
- [Future Plans](#future-plans)
- [Conclusion](#conclusion)


## About Project
To participate in the Neurobagel query federation, datasets must conform to Neurobagel’s data model, so annotating the datasets is necessary to harmonize them for query federation. The project aims to reduce the human effort to manually annotate individual data elements by automating the current annotation tool provided by Neurobagel using Large Language Models (LLMs). The tsv fies uploaded by the users get annotated by the LLMs to obtain the 'TermURLs' corresponding to each element , various other procesing is caried out once the term url is obtained and then the final json file with all the information of all thecolumns of the tsv file is obtained which can then be passed by a human.
The project includes automating the annotation process using LLMs, then integrating the tool into the existing webpage and making changes in the UI accordingly.

## Summary of Work Done
In the first week of our project, the main objective was to process the input TSV file and achieve two outcomes:

    1)Generate a dictionary with key value pairs representing column_header and a string of the column_header concatenated with the column entries.

    2)Create a JSON file with column headers as keys and empty value fields, which would be populated further in the process.

After completing the initial processing, we realized that the project should be divided into two main parts:
    Parsing
    Categorization

My primary focus was on the categorization aspect for quit some time before I startedworking on the react UI..
For categorization, I used open-source LLMs from Ollama and leveraged various tools from LangChain. The goal was to categorize columns (key-value pairs) into six categories:

    Participant ID,
    Session ID,
    Age,
    Sex,
    Diagnosis, and 
    Assessment Tool
The first milestone was successfully categorizing Participant ID, Session ID, Age, and Sex. Following this, we worked on categorizing Diagnosis and Assessment Tool.

Prompt templates were extensively used and proved to be a critical component for the efficient functioning of the project. The prompt template for the first four categories included examples of inputs and how the LLM should respond. This approach worked well for the first four categories, but the LLM became confused when examples for Diagnosis and Assessment were included in the same prompt template. To address this, two additional prompt templates were created—one for identifying Diagnosis and the other for Assessments. These templates included descriptions of the respective categories and instructions to return a "yes" or "no," which was then used for categorization.

<div align="center">
<img src="https://drive.google.com/file/d/1yRDAsOETBYXmCdVUaLS_3YgddYD5WZ7B/view?usp=drive_linkdocs/assets/promptTemplate.png" alt="drawing" width="500"/>
</div> 


The response from the LLM was of the type "AI_model_object," which needed to be converted into a string type for further processing as required by the parsing code to obtain the final result. The workflow of how a key-value pair is checked for its category is illustrated in the flowchart:

<div align="center">
<img src="https://drive.google.com/file/d/1v82_sGfTCHV_VfQifB_yd_Ya3RUWC2Vh/view?usp=sharing" alt="drawing" width="500"/>
</div> 



Tests were written using pytest, and the LLM responses were mocked because GitHub cannot directly call Ollama. The Pydantic library was extensively used to validate the values returned at various steps, ensuring the format and data types were in accordance with the requirements.

Once the Python script was fully functional, we developed a FastAPI service to run it, which was then integrated with a React frontend.

## Contributions

### Pull Requests Issued

- [Create raw json with column headers](https://github.com/neurobagel/annotation-tool-ai/pull/11)
- [Categorization of Participant_id, Session_id](https://github.com/neurobagel/annotation-tool-ai/pull/26)
- [Categorization of Age and Sex Columns](https://github.com/neurobagel/annotation-tool-ai/pull/30)
- [Adding Readme(This was continuosly updated by us)](https://github.com/neurobagel/annotation-tool-ai/pull/35)
- [Assessment Descriptions](https://github.com/neurobagel/annotation-tool-ai/pull/46)
- [Diagnosis Levels with correct API response](https://github.com/neurobagel/annotation-tool-ai/pull/57)
- [UI development](https://github.com/neurobagel/annotation-tool-ai/pull/60)
