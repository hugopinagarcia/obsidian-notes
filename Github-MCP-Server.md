GitHub-MCP vs. ABAP-Knowledgebase-MCP

**1. Erstellter GitHub-MCP (Konzeption & Funktion)** Im Rahmen des Projekts wurde ein dedizierter GitHub-MCP-Server (anbindungfähig über die SAP BTP via SSE-Tunnel) konzipiert und aufgesetzt. Seine Kernfunktion sollte darin bestehen, offizielle SAP-Architekturrichtlinien, Clean-Core-Regeln und Styleguides (wie z. B. aus dem Repository `SAP/styleguides`) in Echtzeit direkt von GitHub abzurufen, um dem KI-Agenten als Live-Wissensbasis zu dienen.

**2. Aktueller Status für den Prototypen** Für die aktuelle Prototypen-Phase wird der GitHub-MCP **vorerst nicht produktiv eingebunden**. Da das bereits vorhandene `ABAP-Knowledgebase-MCP` die entsprechenden Repositories und Leitfäden ebenfalls enthält (offline indiziert), deckt es den Informationsbedarf für das Refactoring vollständig ab und ist für den Entwicklungsbetrieb ausreichend.

**3. Ausblick für den Produktivbetrieb** Für einen späteren Go-Live in die Produktion sollte jedoch evaluiert werden, den GitHub-MCP (oder einen automatisierten Synchronisationsmechanismus) einzusetzen. Dadurch wird sichergestellt, dass der Agent stets auf die absolut neuesten, von SAP aktualisierten Leitfäden zugreift und architektonische Änderungen zeitnah berücksichtigt werden können.






Neurons MCP-Settings:

Description:
GitHub connection via SAP BTP SSE tunnel. Acts as a real-time knowledge base interface for the agent to fetch official SAP Clean ABAP styleguides, RAP patterns, and architectural rules (e.g., from SAP/styleguides) to ensure compliant S/4HANA code refactoring.


Instructions:

You use this GitHub MCP server as your exclusive "Single Source of Truth" for SAP architectural rules and Clean Core guidelines. NEVER search blindly on GitHub; EXCLUSIVELY use the following predefined repositories:

1. FOR CLEAN CORE & NAMING CONVENTIONS:
   - owner="SAP", repo="styleguides"
   - Use path="clean-abap/CleanABAP.md" (or "clean-abap/CleanABAP_de.md") to verify base rules and formatting guidelines.

2. FOR RAP PATTERNS & CODE EXAMPLES:
   - owner="SAP-samples", repo="abap-platform-refcode"
   - owner="SAP-samples", repo="abap-platform-rap-workshops"
   - Use these repositories when you need concrete syntax examples for Behavior Definitions (BDEF), Entity Manipulation Language (EML), or Draft scenarios.

IMPORTANT RULES FOR CALLING THE TOOLS:
1. Parameter Formatting (Mandatory):
   - When using tools like `get_file_contents`, strictly separate the owner and repo fields. Example: owner="SAP", repo="styleguides" (NEVER use "SAP/styleguides" in the repo field).
   - NEVER put double quotes around the values in the "path" field.

OUTPUT PROCESSING AND FORMATTING:
1. Internal Parsing: The downloaded Markdown files from GitHub are massive. NEVER output the raw file content or huge text blocks to the user.
2. Synthesis: Internally analyze the file in your context, extract ONLY the specific architectural rule relevant to the current S/4HANA problem, and use this knowledge to generate the code.
3. Presentation for the User: When presenting the new, refactored code to the user, ALWAYS include a short block (e.g., titled "Architectural Note") at the end. In 1-2 sentences, explain which specific rule from the GitHub styleguides you based your design decision on.
   
   
   MCP-Server-URL:
    https://mcp-github-server.cfapps.eu10.hana.ondemand.com/sse