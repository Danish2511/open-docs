# Day 1 – Requirement

**Owners:** Nancy / Sateesh

---

## 1. Create GI Ticket (Parent) → Assign to Danish

**Reference:**  
https://amwaycloud.atlassian.net/wiki/spaces/IntegrationDocumentMaster/pages/119276613/Canonical+In...

---

## 2. Analysis (**Very Important**)

- Understand the change.
- Identify where the change needs to be made.
- Identify all dependencies before implementing any change.
- Document all findings clearly for future reference and ticket tracking.

---

## 3. Present in COP

- If urgent, set up a call with **Yogendra / Srikanth**
- This step should **not** be a blocker

---

## 4. After GI Ticket Approval in COP

---

## 5. Create GICDCLOUD Schema PR and Deployment

**Reference:**  
https://amwaycloud.atlassian.net/browse/GICDCLOUD-5012

### For New Creation

- **Create a Data Model PR**

  - Define the **Data Model Name** (must be included in the topic name)
  - Define the **Contract Name**  
    _(e.g., `ABGCDM_AddressBook_CDM_AddressBookCDM`)_
  - Use `AmGICodeUtilsApp.Utils:getProto` to generate the proto
    - Remove `_env`
    - Add the required syntax and package

- **Create a Contract PR**

- **Update the Existing PR**
  - Any new field must follow the existing **naming convention**
  - New fields must be added **at the end of the block**
  - **Do not** modify existing proto definitions
  - **Backward compatibility** must be maintained

---

### Note: (Schema Syntax Guidelines)

- **Syntax 2**: Default option for all legacy models.

- **Syntax 3 with optional fields**  
  Use when there is **no dependency on webMethods** (consumer or producer).  
  Requires `protoc.jar` version **3.11**.

- **Special Case (Not Recommended)**
  - **Syntax 3 without optional fields** works with any client, including webMethods.

---

# Day 2 (MWADM Ticket: `20250822_GI-22799`)

## 5. webMethods Canonical

**Canonical IS URL:**  
http://uslxcp89087:5656

- Locate the document where changes are required.
- Ensure all fields are marked as **optional**.
- Add appropriate **comments**.

---

## 6. Canonical Deployer

**URL:**  
http://uslxcp89087:5656/WmDeployer/

- Create a deployment project: `20261216_GI-XXXX`
- Create a set.
- Select the source server: `uslxcp89087`
- Select the required packages.
- Verify all required documents for deployment.
- Resolve dependencies.
- Create the build.
- Create maps and deployment candidates.

---

# Day 3 – ICCLOUD – Container JDA WMS / WM Canonical

**Ticket:**  
https://amwaycloud.atlassian.net/browse/ICCLOUD-851

## Repositories

- **wm-canonical**  
  https://github.com/AmwayCommon/wm-canonical

  - Branches: `Dev` / `Test` / `QA` / `master`

- **wm-bundle-jda-wms**  
  https://github.com/AmwayCommon/wm-bundle-jda-wms
  - Branches: `Development` / `StagingTS` / `StagingQA` / `StagingPD`

---

## Development Steps

- Create a branch in **wm-canonical** using the **MWADM / GICD ticket name**.
- Clone the branch to your local machine.
- Export the build after review and approval.

---

## Package Copy Steps

- **Copy from:**
  ...\myBuild.zip.166\myDeploymentSet\IS\local\Packages{{pkgname}}.zip\ns{{pkgname}}

- **Paste into:**
  ...\MWADM-26825-DV-Demo\wm-canonical\packages{{pkgname}}\ns{{pkgname}}

- Create a **Pull Request (PR)**.

## How to Create a Release Tag

**Repository:**  
https://github.com/AmwayCommon/wm-bundle-jda-wms

- Go to the repository.
- Navigate to **Releases**.
- Click **Draft a new release** and provide a unique release number.

### Notes

- Use **3-digit tags** (e.g., `v1.98`) for **non-production**.
- Use **2-digit tags** (e.g., `v4.5`) for **production**.

---

# Day 4 – EDM (Camel)

## Jira Tickets

- https://amwaycloud.atlassian.net/browse/GICDCLOUD-6356
- https://amwaycloud.atlassian.net/browse/GICDCLOUD-6101

---

## High-Level Flow

- **Integration Artifacts (IA)** → **Jenkins (3 Jobs)** → **Schema Registry** → **EDM**

---

## Repositories & Environments

- **EDM Repository:**  
  https://github.com/AmwayCommon/enterprise-data-models

  - Active branches:  
    `staging_dv`, `staging_ts`, `staging_qa`, `main`

- **ADK Configurations:**  
  https://control-pane.gi-pd.amway.net/home?environment=dv

---

## Important Notes

- Before promoting any **dependent Camel integration**, ensure the **EDM version is updated**.  
  _EDM version updates are handled by the admins._

- A **new EDM version** is required **only when a new data model** is introduced or required for Camel.

---

## Onboarding a New Canonical Model in EDM

---

## Required Updates

### 1. Java Configuration

- **File:** `java/pom.xml`

### 2. Python Artifacts

- **File path:**  
  python/{{datamodelname}}/{{contractname}}\_pb2.py

---

## Generating Python Code

### Preparation

- Clone the **IA repository** locally from the `staging_dev_test` branch and ensure it is up to date.
- Create a new branch from `staging_dv` in the **EDM repository** and clone it locally.

---

### Protobuf Compilation Command (Generic)

```bash
C:\wMServiceDesigner\common\bin\protoc ^
--proto_path=integration-artifacts\{{pkgname}}\protobuf\ ^
--python_out=enterprise-data-models\python\{{datamodelname}}\ ^
integration-artifacts\{{pkgname}}\protobuf\{{contractname}}.proto
```

### Sample Command

```bash
C:\wMServiceDesigner\common\bin\protoc ^
--proto_path=--proto_path=integration-artifacts\abgcdm\protobuf ^
--python_out=--python_out=enterprise-data-models\python\demandorderresponsecdm ^
integration-artifacts\abgcdm\protobuf\ABGCDM_DemandOrder_CDM_DemandOrderResponseCDM.proto
```

---

### Final Step

- Raise a **Pull Request** ref **GICDCLOUD-6356**

- create PR for higher env code promotion in EDM repo.
  - staging_dv -> staging_ts
  - staging_ts -> staging_qa
  - staging_qa -> main
