# SailPoint IdentityIQ Configuration Repository

This repository is a personal practice project for managing SailPoint IdentityIQ configuration objects using source control.

The goal of this repository is to simulate how companies maintain and promote IdentityIQ changes such as Rules, Workflows, Applications, Forms, Task Definitions, and other IIQ objects across environments using Git.

---

## Repository Structure

```text
sailpoint-ssb/
├── config/
│   ├── Application/        # Application XMLs
│   ├── Rule/               # IdentityIQ Rule XMLs with BeanShell source
│   ├── Workflow/           # Workflow XML definitions
│   ├── Bundle/             # Role / Bundle XMLs
│   ├── Form/               # Form XMLs
│   ├── TaskDefinition/     # Task Definition XMLs
│   ├── EmailTemplate/      # Email Template XMLs
│   ├── QuickLink/          # QuickLink XMLs
│   ├── Policy/             # Policy XMLs
│   ├── ObjectConfig/       # ObjectConfig XMLs
│   └── Custom/             # Custom IIQ objects
│
├── src/
│   └── java/               # Custom Java classes
│
├── scripts/
│   ├── export/             # Export helper scripts
│   └── import/             # Import helper scripts
│
├── build/                  # Generated build artifacts
├── env/                    # Environment-specific property files
│   ├── dev.target.properties
│   ├── test.target.properties
│   └── prod.target.properties.example
│
├── build.xml               # Ant / SSB build script
├── build.properties        # Global build properties
├── .gitignore              # Files and folders ignored by Git
└── README.md
```

---

## Purpose

This repo is used to practice a real-world IdentityIQ development workflow:

```text
Make change in DEV IIQ
        ↓
Export the changed IIQ object as XML
        ↓
Place the XML file into the correct repo folder
        ↓
Use Git to review the diff
        ↓
Commit and push the change
        ↓
Import the same XML into higher environments
```

The main purpose is to make Git the source of truth for IdentityIQ configuration.

---

## Common IdentityIQ Objects Managed Here

This repository may contain XML exports for the following IdentityIQ objects:

| Object Type     | Folder                   |
| --------------- | ------------------------ |
| Application     | `config/Application/`    |
| Rule            | `config/Rule/`           |
| Workflow        | `config/Workflow/`       |
| Bundle / Role   | `config/Bundle/`         |
| Form            | `config/Form/`           |
| Task Definition | `config/TaskDefinition/` |
| Email Template  | `config/EmailTemplate/`  |
| QuickLink       | `config/QuickLink/`      |
| Policy          | `config/Policy/`         |
| ObjectConfig    | `config/ObjectConfig/`   |
| Custom Object   | `config/Custom/`         |

---

## Recommended Development Flow

### 1. Get the latest code

```bash
git checkout main
git pull
```

### 2. Create a feature branch

```bash
git checkout -b feature/update-iiq-rule
```

Example branch names:

```text
feature/add-ad-provisioning-rule
feature/update-joiner-workflow
feature/add-access-request-form
bugfix/fix-correlation-rule
```

---

## Exporting Objects from IdentityIQ

When an object is created or updated in IdentityIQ DEV, export that object as XML.

Examples:

```text
Rule: AD Before Provisioning Rule
Workflow: Joiner Workflow
Application: Active Directory
Task Definition: AD Account Aggregation
```

After exporting, place the XML file in the matching folder.

Example:

```text
config/Rule/AD-Before-Provisioning-Rule.xml
config/Workflow/Joiner-Workflow.xml
config/Application/Active-Directory.xml
config/TaskDefinition/AD-Account-Aggregation.xml
```

---

## Updating Existing Files

If the object already exists in Git, replace the existing XML file with the newly exported XML file.

Then run:

```bash
git status
git diff
```

Git will show the exact difference between the previous version and the newly exported version.

Example:

```bash
git diff config/Rule/AD-Before-Provisioning-Rule.xml
```

---

## Adding New Files

If the object is new and does not already exist in Git, add it to the correct folder.

Example:

```text
config/Rule/New-Correlation-Rule.xml
```

Then check status:

```bash
git status
```

Git will show the file as untracked.

---

## Reviewing Changes

Before committing, always review the diff:

```bash
git diff
```

Check for:

* Correct business logic changes
* Correct object name
* No unwanted generated fields
* No passwords or secrets
* No environment-specific values
* No accidental unrelated object changes

---

## Committing Changes

After reviewing the diff:

```bash
git add config/Rule/AD-Before-Provisioning-Rule.xml
git commit -m "Update AD before provisioning rule"
```

For multiple files:

```bash
git add config/
git commit -m "Add new IIQ rules and update workflow configuration"
```

Push the branch:

```bash
git push origin feature/update-iiq-rule
```

Then create a Pull Request in GitHub.

---

## Importing XML into IdentityIQ

After changes are reviewed and approved, the same XML should be imported into the target IdentityIQ environment.

Typical promotion flow:

```text
DEV → TEST → UAT → PROD
```

The same approved XML should be used across environments. Avoid manually recreating the same change in each environment.

---

## Environment Properties

Environment-specific values should be managed separately.

Examples of environment-specific values:

```text
Database URLs
Application hostnames
Service account usernames
Encrypted passwords
API endpoints
IQService hostnames
```

Use placeholder tokens in XML where possible.

Example:

```xml
<entry key="host" value="%%AD_HOST%%"/>
<entry key="port" value="%%AD_PORT%%"/>
<entry key="serviceAccount" value="%%AD_SERVICE_ACCOUNT%%"/>
```

Then define values in environment-specific property files:

```text
env/dev.target.properties
env/test.target.properties
env/prod.target.properties
```

Do not commit real production secrets.

---

## Files That Should Not Be Committed

Do not commit:

```text
build/
dist/
target/
*.class
*.jar
*.war
.DS_Store
real passwords
real private keys
runtime task results
identity data
account/link data
certification data
audit event data
work item data
```

Example `.gitignore`:

```gitignore
build/
dist/
target/
*.class
*.jar
*.war
.DS_Store
*.log

# Local environment files
env/local.target.properties
env/*.secret.properties
```

---

## Best Practices

* Keep Git as the source of truth.
* Make changes in DEV first.
* Export clean XML from IIQ.
* Replace the local XML file in the repo.
* Use `git diff` before committing.
* Do not commit secrets.
* Do not edit PROD directly.
* Use Pull Requests for review.
* Keep object names and file names consistent.
* Commit only related changes together.
* Add clear commit messages.

---

## Example Workflow

Suppose two new Rules were created and one Workflow was updated in IIQ DEV.

Export the objects and place them here:

```text
config/Rule/New-Joiner-Correlation-Rule.xml
config/Rule/AD-Before-Provisioning-Rule.xml
config/Workflow/Joiner-Workflow.xml
```

Check Git status:

```bash
git status
```

Review changes:

```bash
git diff
```

Add files:

```bash
git add config/Rule/New-Joiner-Correlation-Rule.xml
git add config/Rule/AD-Before-Provisioning-Rule.xml
git add config/Workflow/Joiner-Workflow.xml
```

Commit:

```bash
git commit -m "Add joiner rules and update joiner workflow"
```

Push:

```bash
git push origin feature/joiner-workflow-updates
```

Create a Pull Request.

---

## Learning Goals

This repository is intended to practice:

* IdentityIQ XML object management
* Rule and Workflow version control
* Git-based change tracking
* Environment promotion process
* SSB-style project structure
* Clean export/import workflow
* Configuration drift prevention
* Safe handling of secrets and environment-specific values

---

## Notes

This is a personal practice repository and does not contain production IdentityIQ data.

Any real company project should follow the organization’s official release process, security policy, and deployment standards.
# sailpoint-ssb
