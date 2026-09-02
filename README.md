# Employee Onboarding Automation

An automated employee onboarding workflow built using **n8n and Vibe Automations**, with Google Drive, Google Docs, and Google Sheets. The workflow demonstrates folder creation, document and spreadsheet generation, parallel processing, and a Parent–Child Workflow architecture.

## 📌 Project Overview

This project automates the creation of an employee onboarding workspace when a new employee joins an organization.

The workflow automatically:

* Creates a dedicated employee folder in Google Drive
* Creates an onboarding Google Doc
* Updates the document with employee details and an onboarding checklist
* Creates an onboarding Google Sheet
* Adds employee information to the spreadsheet
* Uses parallel processing for document and spreadsheet creation
* Passes data from a Parent Workflow to a Child Workflow
* Demonstrates automated folder cleanup using the Child Workflow

## 🎯 Project Goal

The goal is to reduce repetitive manual work during employee onboarding by automatically creating and organizing the required onboarding resources.

## 💼 Real-Time Use Case

When HR receives a new employee record, the automation can automatically create the employee's onboarding workspace.

For example, when a new employee record is received, the workflow can create a dedicated Google Drive folder containing the employee's onboarding document and spreadsheet.

## 🛠️ Tools Used

* **n8n** - Workflow automation and orchestration
* **Vibe Automations** - Automation workflow environment
* **Google Drive** - Employee folder and file management
* **Google Docs** - Onboarding document creation and updates
* **Google Sheets** - Employee onboarding data management

## 👤 Sample Employee Input

| Field         | Example Value    |
| ------------- | ---------------- |
| Employee ID   | EMP101           |
| Employee Name | Sample Employee  |
| Department    | Data Engineering |
| Joining Date  | 08-Aug-2026      |

## 🔄 Parent Workflow

The Parent Workflow is responsible for preparing the employee onboarding workspace.

### Workflow Steps

**1. Manual Trigger**

Starts the workflow manually for testing.

**2. Edit Fields**

Stores employee information such as:

* Employee ID
* Employee Name
* Department
* Joining Date

**3. Create Folder**

Creates a dedicated Google Drive folder for the employee.

**4. Parallel Processing**

After the folder is created, the workflow splits into two independent branches.

### Branch 1: Google Doc

```text
Create Document
      ↓
Update Document
```

Creates the onboarding document and adds employee information and the onboarding checklist.

### Branch 2: Google Sheet

```text
Create Spreadsheet
      ↓
Append Row
```

Creates the onboarding spreadsheet and adds the employee's onboarding information.

### 5. Merge

The two parallel branches are synchronized using the Merge node.

### 6. Call Child Workflow

After both branches complete, the Parent Workflow passes the Google Drive folder ID to the Child Workflow.

## ⚡ Parallelization Concept

The document and spreadsheet branches are independent tasks. Therefore, both branches are started from the same **Create Folder** node instead of executing strictly one after another.

```text
                    Create Folder
                    /           \
                   /             \
          Create Document    Create Spreadsheet
                 ↓                   ↓
         Update Document       Append Row
                  \                 /
                   \               /
                       Merge
                         ↓
                Call Child Workflow
```

This demonstrates parallel workflow execution and improves the structure of the automation.

## 📄 Google Doc Output

The generated onboarding document contains:

```text
EMPLOYEE ONBOARDING

Employee ID: EMP101
Employee Name: Sample Employee
Department: Data Engineering
Joining Date: 08-Aug-2026

Welcome to the team!

Onboarding Checklist:
- Complete HR documentation
- Complete company orientation
- Set up required tools
- Meet the team
```

## 📊 Google Sheet Output

The onboarding spreadsheet contains structured employee information:

| Employee ID | Employee Name   | Department       | Joining Date | Status     |
| ----------- | --------------- | ---------------- | ------------ | ---------- |
| EMP101      | Sample Employee | Data Engineering | 08-Aug-2026  | Onboarding |

## 🔗 Parent–Child Workflow Architecture

The Parent Workflow calls a separate Child Workflow after the document and spreadsheet branches have completed.

The Parent Workflow passes the Google Drive folder ID as workflow input.

```text
Parent Workflow
      ↓
Complete onboarding resources
      ↓
Call Child Workflow
      ↓
Pass folder_id
      ↓
Child Workflow
```

This architecture demonstrates how larger automations can be separated into reusable workflows.

## 🧩 Child Workflow

The Child Workflow is triggered using:

**Execute Sub-workflow When Executed by Another Node**

Current structure:

```text
Execute Sub-workflow
        ↓
Receive folder_id
        ↓
Google Drive - Delete Folder
```

The folder ID received from the Parent Workflow is mapped dynamically.

```text
{{$json.folder_id}}
```

The Delete Folder operation can then use this ID to remove the employee onboarding folder and its contents according to the configured Google Drive operation.

## 🧪 Testing & Validation

The workflow was tested with sample employee data to validate:

* Employee information is processed correctly.
* A dedicated Google Drive folder is created.
* The Google Doc is created successfully.
* Employee details and the onboarding checklist are added to the document.
* The Google Sheet is created successfully.
* Employee information is added to the spreadsheet.
* Both parallel branches complete successfully.
* The Merge node synchronizes the branches.
* The Parent Workflow successfully calls the Child Workflow.
* The folder ID is passed dynamically to the Child Workflow.
* The Child Workflow performs the configured Google Drive operation.

## 🚀 Future Improvements

* Add an HR form as the workflow trigger
* Send automated welcome emails to new employees
* Add Slack or Microsoft Teams notifications
* Create automated onboarding task tracking
* Add manager approval steps
* Add onboarding reminders
* Integrate with HR management systems
* Add error handling and workflow notifications
* Create reusable onboarding templates
* Add automated status tracking

## 📌 Workflow Summary

```text
Manual Trigger
      ↓
Edit Fields
      ↓
Create Folder
   ↙       ↘
Create      Create
Document    Spreadsheet
   ↓            ↓
Update       Append Row
Document        ↓
   ↘           ↙
       Merge
         ↓
Call Child Workflow
         ↓
Delete Folder
```

## 👩‍💻 Project Highlights

This project demonstrates practical experience with:

* Workflow automation
* n8n
* Google Workspace integrations
* API-based workflow concepts
* Parallel processing
* Data mapping
* Parent–Child Workflow architecture
* Dynamic workflow inputs
* Automated document generation
* Automated spreadsheet management
* Workflow testing and validation
