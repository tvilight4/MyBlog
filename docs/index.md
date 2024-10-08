# Final Evaluation Report for GSOC 2024


![Screenshot from 2024-08-25 16-39-20](https://github.com/user-attachments/assets/8986652e-2b46-41c5-95c6-0de50a5b8701)


## Details

|  |  |
| --- | --- |
| Name | [Tvisha Vedant](https://github.com/tvilight4) |
| Organisation | [Neurobagel](https://neurobagel.org/), [INCF](https://www.incf.org/)  |
| Mentor | [Arman Jahanpour](https://github.com/rmanaem), [Sebastian Urchs](https://github.com/surchs),[Alyssa Dai](https://github.com/alyssadai),[Brent Mcpherson](), [Jean-Baptiste Poline ](https://github.com/jbpoline) |
| Project | [LLM-assisted tool to annotate research data with machine-understandable, semantic data dictionaries](https://summerofcode.withgoogle.com/programs/2024/projects/RbOlafUP) |
| GitHub | [annotation-tool-ai](https://github.com/neurobagel/annotation-tool-ai) |
|Proposal Link | [Proposal](https://drive.google.com/file/d/14q-lONhCMoZImAJuH0ZKCXXltvs84h4q/view?usp=sharing) |


## Table of Contents

- [About Project](#about-project)
- [Summary Of Work Done](#summary-of-work-done)
- [Contributions (PRs)](#contributions)
- [Code That Didnt merge](#code-that-did-not-get-merged)
- [Future Plans](#future-plans)
- [Conclusion](#conclusion)


## About Project
To participate in the [Neurobagel](https://neurobagel.org/) query federation, datasets must conform to Neurobagel’s data model, so annotating the datasets is necessary to harmonize them for query federation. The project aims to reduce the human effort required to manually annotate individual data elements by automating the current [annotation tool](https://neurobagel.org/annotation_tool/) tool provided by Neurobagel using Large Language Models (LLMs). The TSV files uploaded by the users get annotated by the LLMs to obtain the 'TermURLs' corresponding to each element, various other processing is carried out once the TermURL is obtained, and then the final JSON file with all the information of all the columns of the TSV file is generated, which can then be reviewed by a human.

The project includes automating the annotation process using LLMs, integrating the tool into the existing webpage, and making changes in the UI accordingly.


## Summary of Work Done

In the first week of our project, the main objective was to process the input TSV file and achieve two outcomes:

    1)Generate a dictionary with key-value pairs representing column_header and a string of the column_header concatenated with the column entries.

    2)Create a JSON file with column headers as keys and empty value fields, which would be populated further in the process.

After completing the initial processing, we realized that the project should be divided into two main parts:

    Parsing
    Categorization

As I was working with a colleague on this project, we divided our work. My primary focus was on the categorization aspect for quite some time before I started working on the React UI.

For categorization, I used open-source LLMs from Ollama and leveraged various tools from LangChain. The goal was to make the LLM categorize columns (key-value pairs) into six categories:

    Participant ID
    Session ID
    Age
    Sex
    Diagnosis and 
    Assessment Tool

The first milestone was successfully categorizing Participant ID, Session ID, Age, and Sex. Following this, we worked on categorizing Diagnosis and Assessment Tool.

Working on the categorization part included the selection of the optimal model by testing the responses given by various models and the optimal utilization of the best possible tools from LangChain. The details of which can be found [here](page2.md).

Prompt templates were extensively used and proved to be a critical component for the efficient functioning of the project. The prompt template for the first four categories included examples of inputs and how the LLM should respond. This approach worked well for the first four categories, but the LLM became confused when examples for Diagnosis and Assessment were included in the same prompt template. To address this, two additional prompt templates were created—one for identifying Diagnosis and the other for Assessments. These templates included descriptions of the respective categories and instructions to return a "yes" or "no," which was then used for categorization.

![promptTemplate](https://github.com/user-attachments/assets/2189d6e1-d6c4-4094-81ca-036d58a6685f)



The response from the LLM was of the type "AI_model_object," which needed to be converted into a string type for further processing as required by the parsing code to obtain the final result. The workflow of how a key-value pair is checked for its category is illustrated in the flowchart:


![NB drawio](https://github.com/user-attachments/assets/2553f4c0-2a83-4ef9-97dd-df6515d5009c)





Tests were written using pytest, and the LLM responses were mocked because GitHub cannot directly call Ollama. The Pydantic library was extensively used to validate the values returned at various steps, ensuring the format and data types were in accordance with the requirements.

The entire code was then dockerized. Once the Python script was fully functional, we developed a FastAPI service to run it, which was then integrated with a React frontend. The React frontend plays a crucial role as the final JSON with only a single diagnosis is created only when the user selects a specific diagnosis from the dropdown provided by the UI.

When we were done with a fully functional Python script using Gemma, we also experimented with the OpenAI GPT-4 model, the key to which was provided by our organization for the purpose of studying and comparing the efficiencies of open-source models and closed models.

To understand the details of the entire codebase, how the columns were analyzed for each individual category, and how the React UI aids in the diagnosis annotations, click [here](page3.md).

During the entire course of the project, we documented our work and progress on HackMD and continuously updated the README of our GitHub repository.

Towards the end of the project, we had the opportunity to present our project and work in front of the entire lab, show a live demo with the latest version of our tool, and address various questions regarding the tool we developed.

## Contributions

### Pull Requests Issued

- [Create raw json with column headers](https://github.com/neurobagel/annotation-tool-ai/pull/11)
- [Categorization of Participant_id, Session_id](https://github.com/neurobagel/annotation-tool-ai/pull/26)
- [Categorization of Age and Sex Columns](https://github.com/neurobagel/annotation-tool-ai/pull/30)
- [Adding Readme(This was continuosly updated by us)](https://github.com/neurobagel/annotation-tool-ai/pull/35)
- [Assessment Descriptions](https://github.com/neurobagel/annotation-tool-ai/pull/46)
- [Diagnosis Levels with correct API response](https://github.com/neurobagel/annotation-tool-ai/pull/57)
- [UI development](https://github.com/neurobagel/annotation-tool-ai/pull/60)

## Code That Did not get Merged
I had explored the use of vector stores for giving context to the LLM using LangChain to let the LLM predict the full form of diagnosis levels and identify whether a column is a diagnosis or an assessment tool, but the outputs were not very accurate, and the prompt template approach proved to be more efficient, so that was used for the final product.

Also, our initial approach for diagnosis was for the LLM to predict the diagnosis levels' full form and then give the user an option to continue with it or replace it with a full form of their own choice from a dropdown. However, in order to take a more pragmatic approach, we decided to keep only the dropdown and eliminate that step.


## Future Plans

- Enhance the UI further to include more flexibility of usage for users, making it easier for carrying out the human check and updating the JSON file.
- Integrate the React app with the live annotation tool.
- Work on increasing the accuracy of the LLM by further enhancing the prompt templates.

## Conclusion

It has been an amazing experience for me, running into unforeseen errors and then solving them, learning the entire software cycle of writing production-level code, and learning about pytest and Docker. Everything has helped me gain new knowledge and developed my thought process in a way that will surely help me in my future endeavors. I am extremely grateful to my mentor **Arman Jahanpour**, **Sebastian Urchs**, **Alyssa Dai**, **Brent McPherson**, **Neurobagel**, and **INCF** for allowing me to work on the project and for all of their guidance and support. The tips and suggestions given by them whenever I got stuck always proved to be really helpful for me to progress further in the project. The insights shared by them helped me develop a unique point of view to approach problems. I would also like to thank Google for providing such a great opportunity to all open-source student developers across the globe.

I am excited and look forward to contributing to Neurobagel and working with the organization on any future endeavors!!

**Thanks and Regards,**

**Tvisha Vedant**
