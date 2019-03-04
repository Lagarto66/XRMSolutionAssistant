# XRMSolutionAssistant
A .NET Standard assembly offering tooling to assist with the management of exported Microsoft CRM solution files and reducing noise in a multi-developer, branched environment.
## Available Tools
- Entity OTC Aligner
- Version Reset
- Workflow Guid Aligner
- XML Sorter
### Overview
#### Entity OTC Aligner
For versions of CRM < 9, each custom entity was assigned a code upon installation. Between different CRM Organizations, this value would be different. Even if sourced from the same solution. This tool allows the code to be set to a known value when extracted.
#### Version Reset
Many items within a CRM solution retain the version that they were initially installed with on a developer instance. To provide predictable, consistent source control files, this tool will reset this to 0.0.0.0 for the following items
- Entity.xml
- Solution.xml
- Reports [TODO]
- Dashboards [TODO]
- WebResources [TODO]
#### Workflow Guid Aligner
Under some circumstances, the xaml for a Workflow may contain ``<Variable x:...>`` elements with Guids that are generated upon installation and therefore different every time the solution is extracted. This tool will replace those Guids with a predictable value.
#### XML Sorter
Due to the nature of the *SolutionPackager.exe* tool, nested elements may be written in an unpredictable order and create noise in source control change. This tool will alpha order those elements to maintain consistency across different extracts.
## Usage
### General
All the tools are contained in the *WARP.XrmSolutionAssistant.dll* which may be called from a console application. *Runner* is a console application with every tool implemented.

All the tools are in the `` WARP.SolutionAssistant `` namespace.

All the tools work on a folder structure that has been created from the *SolutionPackager.exe* console application from the CRM SDK.

Alongside the assembly is ``settings.json`` that provides information to some of the tooling.
### Runner
Runner is an example console application which may be used for a quick-start and implements all the tools in the assistant assembly.
```
WARP.XrmSolutionAssistant.Runner.exe /<folder>
```
Where *folder* is the path to the extracted solution file.

### Entity OTC Aligner
Modify your ``settings.json`` to contain a collection of ``EntityTypeCodes`` as below:
```javascript
{
  "EntityTypeCodes": [
    {
      "EntityLogicalName": "warp_customentity1",
      "TypeCode": 10029
    },
    {
      "EntityLogicalName": "warp_customentity2",
      "TypeCode": 10028
    }
  ]
}
```
Implementation
```csharp
            var entityAligner = new SolutionEntityAligner(folder);
            entityAligner.Execute();
```
### Version Reset
Implementation
```csharp
            var solutionVersionResetter = new SolutionVersionReset(folder);
            solutionVersionResetter.Execute();
```
### Workflow Guid Aligner
Implementation
```csharp
            var workflowGuidAligner = new SolutionWorkflowGuidAligner(folder);
            workflowGuidAligner.Execute();
```
### XML Sorter
Implementation
```csharp
            var sorter = new SolutionXmlSorter(folder);
            sorter.Execute();
```
