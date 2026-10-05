<!-- GLOBAL_AGENT_POLICY_V1_START -->
# Global Agent Policy

Diese Regeln gelten repoübergreifend für alle AI-Coding-Agents des Owners (insbesondere Claude Code, Codex und GitHub Copilot). Repo-spezifische Regeln ergänzen diese Policy, dürfen sie aber nicht stillschweigend widersprechen.

## Arbeitsmodell
- Das gemeinsame GitHub Project ist der globale Arbeits-Hub.
- Sichtbare Status: **Backlog → Todo → In Progress → Review → Blocked → Done**.
- **Backlog** = noch nicht eingeplant.
- **Todo** = bewusst als nächstes zu bearbeiten / vom Owner freigegeben.
- **In Progress** = Agent arbeitet aktiv oder ein technischer Prozess läuft.
- **Review** = der Owner soll etwas Sichtbares, Inhaltliches oder eine Entscheidung prüfen.
- **Blocked** = Input, Berechtigung oder Abhängigkeit fehlt.
- **Done** = technisch abgeschlossen und geprüft. Zugehöriges Issue schließen mit Abschlussgrund `completed`, sofern das Repo keine dokumentierte Ausnahme hat.
- Done-Karten bleiben 24 Stunden sichtbar und werden danach nur aus dem Project entfernt. Das geschlossene Issue bleibt erhalten.
- Wenn ein Issue bearbeitet wird, muss sein Board-Status während der Arbeit korrekt mitgeführt werden. Nicht nur Code ändern und das Issue unverändert liegen lassen.
- Repo-Labels wie `repo:Telli`, `repo:Soundbook` oder `repo:Millennial` werden verwendet, wenn sie für das Repo konfiguriert sind.

## Verhalten während eines Issues
1. Vor Beginn Issue und lokale Agent-/Repo-Dokumentation lesen.
2. Bei Arbeitsbeginn: Issue/Karte auf **In Progress**.
3. Änderungen implementieren und mit den repo-spezifischen Checks testen.
4. Wenn der Owner etwas beurteilen muss: **Review** und im Issue klar dokumentieren:
   - was geprüft werden soll,
   - wo es sichtbar ist,
   - woran Erfolg erkennbar ist,
   - wie der Owner antworten soll.
5. Fehlt Input oder eine Berechtigung: **Blocked** mit klarer Frage.
6. Wenn vollständig fertig und geprüft: **Done**, Issue als `completed` schließen, sofern keine Repo-Regel etwas anderes verlangt.
7. Kurze Abschlussmeldung. Keine langen Prozessberichte ohne Nutzen.

## Sicherheits- und Qualitätsregeln
- Keine Secrets, Tokens oder Zugangsdaten in Code, Issues, Kommentaren oder Dokumentation schreiben.
- Keine Veröffentlichung, destruktive Aktion, Löschung, kostenpflichtige Aktion oder Änderung kritischer Berechtigungen ohne ausdrückliche Freigabe des Owners.
- Keine Tests behaupten, die nicht ausgeführt wurden.
- Bestehende Repo-Konventionen, Design-Systeme, Release-Regeln und Sicherheits-Gates haben Vorrang vor generischen Agent-Gewohnheiten.
- Keine unnötige PR-/Branch-Zeremonie einführen, wenn das Repo ausdrücklich einen anderen Workflow dokumentiert.

## Dokumentations-Konsistenz ist verpflichtend
Änderungen an **Workflow, Zusammenarbeit, Board-/Issue-Logik, Agent-Verhalten, Automatisierung, Freigaben, Statusmodell, Rollen oder Sicherheits-Gates** gelten erst als vollständig, wenn alle betroffenen Stellen aktualisiert wurden.

Das bedeutet insbesondere:
- `AGENTS.md`
- `CLAUDE.md`
- `.github/copilot-instructions.md`
- relevante Dateien unter `docs/`
- Automations-/Workflow-Dateien unter `.github/workflows/`
- relevante Helper-Skripte für Board, Issues oder Agents
- Tests, die das Verhalten absichern
- Entscheidungs-/Handover-Dokumentation, falls vorhanden

**Kein Agent darf nur seine eigene Instruktionsdatei aktualisieren.** Wenn eine Änderung Claude, Codex oder Copilot gleichermaßen betrifft, müssen die entsprechenden Agent-Dokumente konsistent mitgezogen werden.

Vor Abschluss einer solchen Änderung:
1. nach veralteten Begriffen/Regeln suchen,
2. widersprüchliche Dokumentation korrigieren,
3. Tests/Checks anpassen,
4. erst danach das Issue auf Done setzen.

## Repo-spezifische Regeln
Nach dieser globalen Policy immer die lokalen Repo-Regeln lesen. Bei Konflikten gilt die strengere bzw. spezifischere Repo-Regel, außer der Owner weist ausdrücklich etwas anderes an.
<!-- GLOBAL_AGENT_POLICY_V1_END -->


## Global board sync after agent operations

After an agent creates, closes, reopens, labels, or otherwise changes one or more GitHub issues in a way that affects the shared Project, it must trigger **exactly one** global board sync at the end of that logical batch.

Preferred command when shell access is available:

`gh workflow run board-sync.yml --repo Broles/morning-briefing`

Rules:
- Batch multiple issue operations first, then trigger the sync once.
- Do not trigger a sync after every single issue in a multi-issue batch.
- If the repo provides a direct board helper that already updates the shared Project immediately, use that helper during the task; still trigger one global sync at the end only when repo/project consistency may have changed.
- If the agent environment cannot dispatch GitHub Actions, do not fake success. Document that the hourly fallback will reconcile the board.
- The scheduled Board Sync is only a safety net and runs hourly; agents should not rely on it for normal issue creation/update workflows.
