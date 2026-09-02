 AgentAuditPro — Cadrage vertical slice
1. Utilisateur. Un consultant en cabinet de conseil en conformité, qui utilise l'outil pour ses missions d'audit ; le dirigeant client (non expert) reçoit ensuite une synthèse vulgarisée du rapport et les experts juridiques qui interviennent en cas de nécessité pour les validations expertises humaines
2. Friction. Actuellement la vérification est manuelle, compléter un dossier de conformité RGPD/AI Act est lent, sujet à oubli de contrôle, et sans trace reproductible d'une mission à l'autre.
3. Résultat attendu. Un rapport d'écart (gap analysis) par contrôle (conforme / non-conforme / à vérifier / information insuffisante), avec citation de la source et de l'article réglementaire avec la date d’application, en synthèse vulgarisée avec les recommandations pour être aux normes
4. Métrique métier. Traçabilité des verdicts : 100% des verdicts rattachés à une source documentaire précise et à l'article réglementaire correspondant.
5. Données, documents ou APIs réellement disponibles. Un corpus synthétique de documents fictifs (registre de traitement, DPIA, documentation technique) et un référentiel de contrôle sourcé sur les articles RGPD/AI Act réels ; données de précédents client audités (entités, controls, non_confirmities)

6. Refus / validation humaine. Document illisible ou absent, ambiguïté sur la classification du système, verdict non-conforme sur un contrôle à fort impact, ou score de confiance d'extraction sous le seuil. 
7. Verticale candidate : Agent. L'orchestrateur choisit à l'exécution, selon les documents réellement observés, quel outil appeler ensuite (extraction, court-circuit vers un refus, vérification de classification), au sein d'un jeu d'outils fermé et de sorties contraintes par schéma.

Les 3 cas:
Nominal: conforme
Ambigu: à vérifier : nécessite une intervention humaine
invalide: non-confome

