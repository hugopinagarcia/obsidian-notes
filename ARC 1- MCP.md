- https://github.com/arc-mcp/arc-1/

-----
# Dokumentation: Integration der ARC-1 Skills in den Syskopilot - CleanCore Agent

## 1. Ausgangslage und Motivation
Im Repository des **ARC-1 MCP Servers** existiert ein Ordner namens `Skills`. Dieser Ordner enthält verschiedene Markdown-Dateien (`.md`), die als vorgefertigte, hochspezifische Prompts und Anleitungen für bestimmte Themengebiete dienen (z.B. Refactoring, CleanCore-Prinzipien, Erstellung von RAP-Services).

Es wurde festgestellt, dass der Agent, der auf die MCP-Tools zugreift, diese   Skill-Dateien nicht automatisch oder direkt aufruft. Um das wertvolle Wissen und die Best Practices aus diesen Dateien dennoch nutzbar zu machen, musste ein Weg gefunden werden, sie dem Agenten zur Verfügung zu stellen.

## 2. Architekturentscheidung
Anstatt den MCP-Server so umzubauen, dass er Dateiinhalte dynamisch als Tool-Response liefert, wurde entschieden, die **Reply Neurons Agent**-Plattform (spezifisch den internen "Syskopilot - CleanCore Agent") direkt zu nutzen. Die Markdown-Dateien werden als **Knowledgebase (Wissensdatenbank)** in den Client integriert.

**Warum dieser Weg? (Vorteile & Begründung)**
* **Direkter Kontext:** Der Agent hat nativen Zugriff auf seine Knowledgebase und kann diese bei semantischen Ähnlichkeiten zum User-Prompt direkt durchsuchen (RAG - Retrieval-Augmented Generation). Das Vorwissen ist da, bevor überhaupt ein Tool aufgerufen wird.
* **Wartbarkeit & Skalierbarkeit:** Kollegen können bestehende Skills einfach über die UI aktualisieren oder neue `.md`-Dateien per Upload hinzufügen, ohne den Code des MCP-Servers oder des Agenten anfassen zu müssen.
* **Trennung von Tooling und Wissen:** Der MCP-Server bleibt fokussiert auf ausführbare Tools und Aktionen (Separation of Concerns), während statisches Wissen, Methodiken und Guidelines im Agenten selbst verankert sind.

## 3. Umsetzung und Implementierung
Die Integration erfolgte in drei wesentlichen Schritten:

### Schritt 1: Extraktion der Skill-Dateien
Die relevanten Markdown-Dateien (`.md`) wurden aus dem `Skills`-Ordner des ARC-1 Repositories gesammelt. 

### Schritt 2: Upload in die Knowledgebase
Diese gesammelten `.md`-Dateien wurden über die "Upload"-Funktion der Reply Neurons Plattform in den "Syskopilot - CleanCore Agent" hochgeladen. Sie fungieren nun als primäre Wissensquelle für spezifische Arbeitsabläufe.

### Schritt 3: Anpassung des System Prompts
Damit der Agent auch weiß, *wann* und *wie* er diese Knowledgebase nutzen soll, wurde der System Prompt des **CleanCore-Agenten** entsprechend erweitert.

**Logik im Prompt:**
Der Agent wurde explizit angewiesen, bei bestimmten Themen (wie z.B. CleanCore, RAP-Services, Refactoring) *zwingend* zuerst in seiner Knowledgebase nachzuschauen, ob es dazu eine spezifische Markdown-Anleitung (Skill) gibt. Falls ja, soll er die dort definierten Schritte, Guidelines und Prompts als strikte Grundlage für seine weiteren Antworten und Tool-Aufrufe nutzen.

