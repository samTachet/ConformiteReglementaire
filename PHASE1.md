# Conformité Réglementaire : Phase 1

## Vertical slice (trajet complet mais étroit)
### Utilisateur, friction, résultat, métrique
Pour répondre à la formulation 
>Pour `[utilisateur]`, réduire `[friction]` en produisant `[résultat]`, mesuré par `[métrique]`.

Nous pouvons l'adapter ici en : 
>Pour `[l'auditeur]`, 
>réduire `[lors de la préparation de l'audit d'une entité, le temps passé à rassembler manuellement, les règlements actuellement en vigueur, les risques connus et les contrôles déjà réalisés, dispersés dans plusieurs sources]`
>en produisant `[une liste priorisée des règlements applicables à la date de la mission]`,
>mesuré par `[rapidité de constitution du dossier et le taux de règlements correctement filtrés]`

### Contrat : entrée, sortie, refus
- **entrée :** un message (une phrase) à auditer
- **sortie:** liste de règlementation avec {regulation_id, code, domain, criticality (HIGH, MEDIUM, LOW), validity (valide|abrogé), status, comment (explication de ambiguité / refus)}
- **refus:**  si aucun règlement n'est trouvé

### Mocks
- Le client LLM est mocké
- Les données sont également mockées.

### 3 cas : nominal, ambigu, invalide
**cas nominal:** on trouve un domaine avec ses réglements applicables
```
python -m src.cli "Nous n'avons pas de DPO désigné mais traitons les données de 800 clients."

[{"regulation_id": "REG-008", "code": "DPO-2023", "domain": "DPO", "criticality": "LOW", "validity": "valide", "status": "OK"}, 
{"regulation_id": "REG-011", "code": "DPO-2024", "domain": "DPO", "criticality": "LOW", "validity": "valide", "status": "OK"}]
```

**cas ambigu:** on trouve plusieurs domaines applicables
```
python -m src.cli "Nous perdons quelques données après un test de plan de continuité."

[{"status": "needs_review", "comment": "Plusieurs domaines trouvés : DPO, business_continuity, gouvernance"}]
```
**cas invalide:** pas de domaine / règlementation applicable
```
python -m src.cli ""
[{"status": "needs_review", "comment": "Aucune réglementation correspondante trouvée."}]
```

## Tools
Voir [TOOLS.md](TOOLS.md) pour les tools identifiés.