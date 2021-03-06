# Integrate DPG and ApiView with SDK Automation Pipeline

As the demo of integrating DPG and ApiView with SDK automation pipeline get a lot of praise, it's expected to make it for production.
This document describes the workflows and lists the work items which needs us to finish.

## Workflow

### Workflow in Demo
![wf](images/workflow.drawio.png)

When service team creates PR in swagger repository, the PR triggers Swagger ApiView Plugin and SDK generation pipeline.

Swagger ApiView generates swagger ApiView diff, and then there is comment added in swagger PR, which contains the link to swagger ApiView diff.

SDK Automation Pipeline generates SDKs and create PR for each SDK. The PR in SDK triggers the ci pipeline, and the ci pipeline generates ApiView. At last, there will be a comment in SDK PR, which contains the link to SDK ApiView diff.

### New Workflow used for Production 
![nwf](images/newWorkflow.drawio.png)

The new workflow used for production is different with the one in demo. In the workflow of demo, the ApiView is integrated in ci pipeline of SDK PR. In new workflow for production, the SDK ApiView will be integrated in SDK automation. Also, there will be a comment about SDK ApiView in Swagger PR.

## Work Items

### ApiView Integration

| Scope                       | Work Item                                                                                | Description                                                                                                                                                      | Owner       | ETA  |
|-----------------------------|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|------|
| Swagger ApiView             | Swagger ApiView Integration                                                              | Generate Swagger ApiView diff When swagger PR is created                                                                                                         | Ruoxuan     | 5/31 |
| SDK ApiView Integration     | Fork SDK repositories to bot account `azure-sdk`, and sync the main branch automatically | ApiView asks the SDK PR created by SDK automation pipeline should be in Azure/Microsoft Org because of microsoft policy                                          | Wes/Praveen | 4/15 |
| SDK ApiView Integration     | Integrate SDK ApiView in SDK automation pipeline                                         | ApiView is generated in ci pipeline of SDK PR in demo. But we hope to move it to SDK automation pipeline, and there will be comment about ApiView in swagger PR. | Wei         | 4/20 |
| SDK ApiView Integration     | Change SDK automation pipeline to create PR in `azure-sdk`'s sdk repositories            | As the ApiView only can be integrated in PRs which is from Azure org to Azure org, we need to migrate sdk automation pipeline to use `azure-sdk` account         | Wei         | 4/18 |

### Integrate DPG in SDK Automation Pipeline

| Scope   | Work Item                                          | Description                                                                                                                                                                                                               | Owner          | ETA  |
|---------|----------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|------|
| JS      | Update automation script to adapt latest codegen   | Codegen has some new changes in generating codes, and existing automation script needs some change to adapt it                                                                                                            | Wei            | 4/15 |
| JS      | Update codegen to use autorest security scopes     | Current codegen still uses self-defined option `credential-scopes`, which should be moved to use the one defined by autorest                                                                                              | Qiaoqiao/Mary  | 4/15 |
| JS      | Clear files in `src` folder in re-generating codes | Current codegen cannot clear `src` folder in re-generating codes, which leads to build failure because some previous file should be deleted                                                                               | Qaioqiao/Mary  | 4/15 |
| JAVA    | Simplify `readme.java.md`                          | Current `readme.java.md` has some duplicated information, which is also contained in `readme.md`. So we need to remove them.                                                                                              | Weidong        | 4/15 |
| Python  | Extend automation script to handle multi-package   | Automation script in python only can handle RPs with one package, such as `deviceupdate`. It's expected to handle multi-packages in a RP, such as purview                                                                 | Yuchao         | 4/15 |
| .Net    | Extend automation script to handle multi-package   | Automation script in .net only can handle RPs with one package, such as `deviceupdate`. It's expected to handle multi-packages in a RP, such as purview                                                                   | Crystal        | 4/15 |
| All SDK | Handle manual codes in automation script           | Sometimes, manual codes (grow up codes, tests, samples) may leads to build failure in re-generating codes, which may block us getting SDK PR and ApiView. So could we ignore/delete manual tests and samples in building? | All SDK Owners | 4/15 |
