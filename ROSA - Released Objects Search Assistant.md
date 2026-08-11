https://github.com/ClementRingot/ROSA


## 1. What is ROSA and What Does It Do for the Project?

The **ROSA (Clean Core) MCP Server** is a backend service that acts as the primary compliance and terminology engine for the Clean Core AI Agent.

Within the context of the Clean Core refactoring project, ROSA serves as the bridge between legacy code analysis and modern SAP cloud standards. Specifically, it allows the AI agent to:

- **Validate Compliance:** Instantly check if legacy SAP objects (e.g., direct reads on **MARA** or **BSEG**, or usage of classic BAPIs) are Clean Core compliant.
    
- **Retrieve Alternatives:** Provide official remediation steps and suggest modern, public alternative APIs (e.g., Released CDS views or RAP Business Objects) when non-compliant objects are found.
    
- **Search the Catalog:** Search the SAP object catalogue directly.
    
- **Define Terminology:** Look up standard SAP abbreviations to help the agent understand undocumented legacy code.
    

## 2. Completed Project Requirements

Based on the agent's task list, the integration of ROSA successfully fulfills (or directly supports) the following requirements:

### ✅ Fully Completed by ROSA

- **Released API Catalog:** ROSA acts as the live catalog interface, allowing the agent to query and discover modern released SAP objects.
    
- **Fill Knowledge Base with API-Allowlist:** ROSA dynamically provides the allowlist by offering compliant public API alternatives whenever the agent encounters a legacy object.
    


## 3. How ROSA Was Implemented on SAP BTP

ROSA was deployed to SAP BTP Cloud Foundry using the **MTA from source** method.

During the implementation, several Cloud Foundry buildpack and caching issues were encountered and resolved. The critical fix required modifying the build configuration to ensure the Node.js runtime correctly installed the required NPM packages on the BTP container.

### Step-by-Step Implementation Details:

**1. Configuration Fix (`mta.yaml`):** The `mta.yaml` file was modified to ensure the `package.json` file was included in the final archive. The `build-result: dist` parameter was explicitly removed so the Cloud Foundry Node.js buildpack could successfully run `npm install` and `npm start` on the server.

YAML

```
modules:
  - name: rosa
    type: nodejs
    path: .
    parameters:
      buildpack: nodejs_buildpack
      memory: 256M
      disk-quota: 512M
      health-check-type: http
      health-check-http-endpoint: /health
    properties:
      NODE_ENV: production
      TRANSPORT: http
    build-parameters:
      builder: npm
    requires:
      - name: rosa-xsuaa
```

**2. Compilation and Build:** The project dependencies were synchronized locally to lock in the correct upstream package name (`@rosa-mcp/server`), compiled, and then packaged using the Cloud MTA Build Tool (`mbt`).

Bash

```
npm ci
npm run build
mbt build
```

**3. Deployment to BTP:** A previous, crashed instance was forcefully deleted to clear the Cloud Foundry staging cache. The newly generated, fully-packaged `.mtar` archive was then deployed, automatically creating and binding the necessary XSUAA security service.

Bash

```
cf delete rosa -r -f
cf deploy mta_archives/rosa_1.14.1.mtar
```

**4. Post-Deployment Authentication Setup:** To ensure the AI agent (MCP client) does not have to re-register its OAuth credentials every time the ROSA server restarts, a stable signing secret was injected into the environment.

Bash

```
cf set-env rosa DCR_SIGNING_SECRET "<generated-base64-secret>"
cf restage rosa
```
