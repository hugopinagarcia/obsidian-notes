GitHub-MCP vs. ABAP-Knowledgebase-MCP

**1. Erstellter GitHub-MCP (Konzeption & Funktion)** Im Rahmen des Projekts wurde ein dedizierter GitHub-MCP-Server (anbindungfähig über die SAP BTP via SSE-Tunnel) konzipiert und aufgesetzt. Seine Kernfunktion sollte darin bestehen, offizielle SAP-Architekturrichtlinien, Clean-Core-Regeln und Styleguides (wie z. B. aus dem Repository `SAP/styleguides`) in Echtzeit direkt von GitHub abzurufen, um dem KI-Agenten als Live-Wissensbasis zu dienen.

**2. Aktueller Status für den Prototypen** Für die aktuelle Prototypen-Phase wird der GitHub-MCP **vorerst nicht produktiv eingebunden**. Da das bereits vorhandene `ABAP-Knowledgebase-MCP` die entsprechenden Repositories und Leitfäden ebenfalls enthält (offline indiziert), deckt es den Informationsbedarf für das Refactoring vollständig ab und ist für den Entwicklungsbetrieb ausreichend.

**3. Ausblick für den Produktivbetrieb** Für einen späteren Go-Live in die Produktion sollte jedoch evaluiert werden, den GitHub-MCP (oder einen automatisierten Synchronisationsmechanismus) einzusetzen. Dadurch wird sichergestellt, dass der Agent stets auf die absolut neuesten, von SAP aktualisierten Leitfäden zugreift und architektonische Änderungen zeitnah berücksichtigt werden können.






Neurons MCP-Settings:

Description:
GitHub connection via SAP BTP SSE tunnel. Acts as a real-time knowledge base interface for the agent to fetch official SAP Clean ABAP styleguides, RAP patterns, and architectural rules (e.g., from SAP/styleguides) to ensure compliant S/4HANA code refactoring.


Instructions:

