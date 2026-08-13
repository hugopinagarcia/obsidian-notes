- https://github.com/arc-mcp/arc-1/
- https://github.com/arc-mcp/arc-1-lsp

-----
### Architektur-Entscheidung: ARC-1 (Standard) vs. arc-1-lsp

Als ich das Projekt übernommen habe, lief der **Standard ARC-1** bereits. Ich habe mir die Alternative **`arc-1-lsp`** angeschaut, mich aber entschieden, beim bestehenden Setup zu bleiben. Hier ist kurz das Warum:

**Was macht der arc-1-lsp?** Er nutzt den SAP Language Server (wie in Eclipse) und bietet Live-Feedback (ATC-Checks) direkt beim Code-Schreiben. Klingt nützlich, hat aber zu viele Haken.

**Warum der Standard ARC-1 für uns völlig reicht:**

- **Stabilität:** Der Standard ARC-1 nutzt offizielle SAP-APIs und läuft absolut verlässlich. Die LSP-Version ist eher ein experimentelles "Proof of Concept" mit fragilen Workarounds (wie Login über unsichtbare Browser).
    
- **Die KI braucht kein Live-Feedback:** Unser Agent kann den fertigen Code einfach per `SAPActivate` ans System schicken. Wenn es Syntax- oder ATC-Fehler gibt, liefert ARC-1 das Fehler-Log zurück und die KI korrigiert sich im nächsten Versuch selbst. Dieser Trial-and-Error-Loop reicht völlig.
    
- **Keine Lizenz-Kopfschmerzen:** Für den LSP-Server müssten wir proprietäre SAP-Dateien per Hand aus Eclipse extrahieren und auf den Server legen. Das ist bei KI-Nutzung eine rechtliche Grauzone, die vermieden werden kann.
    

**Fazit:** Der aktuelle ARC-1 macht genau das, was wir fürs Refactoring brauchen. Ein Wechsel auf die LSP-Variante würde nur das Setup verkomplizieren und unnötige Risiken bringen.

----------------------------------------------------------------

