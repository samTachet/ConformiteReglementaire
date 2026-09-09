# Tools utilisés sur le projet

## Tool search_applicable_regulations : identification des règlements avec la référence de l'article et sa criticité
### 1. objectif
Pour analyser un écart potentiel, l'auditeur doit d'abord savoir quels règlements et quels articles sont susceptibles de s'appliquer à la pratique décrite dans un texte.

Le tool doit renvoyer la liste factuelle des règlements avec les articles et criticités associés. S'il ne trouve aucune correspondance ou si plusieurs règlements peuvent s'appliquer, il doit renvoyer tous les règlements trouvés.

**En résumé :**
- Si un ou plusieurs règlements/articles correspondent au texte, renvoyer la liste
- Si aucune correspondance n'est trouvée, renvoyer une liste vide
- Si plusieurs règlements sont plausibles, les renvoyer tous

<span style="color:red">Le tool ne doit pas :
- donner une conclusion de conformité ou non de la pratique
- filtrer les règlements de lui-même. Il doit renvoyer la liste des règlements applicables
- accéder aux données propres aux entités auditées (entities.csv, non_conformities.csv, ...)
- accéder à `donnees/jeux_evaluation/eval_cases.csv`
</span>

### 2. entrées
- request_input : phrase décrivant une pratique opérationnelle dans une entité
### 3. sorties
- liste de règlements avec les champs {regulation_id, article, criticity}
### 4. permissions
- Lecture du fichier `donnees/donnees_tabulaire/regulations.csv` et des sources documentaires du répertoire `donnees/sources_documentaires/reglement_rgd_2024.md` et  `donnees/sources_documentaires/directive_securite_si.md`.
- Pas d'écriture
- Pas d'appel d'api externe

### 5. erreurs possibles
- Si `request_input` est vide : générer une erreur `invalid_input`
- Si les fichiers de référence n'existent pas / accessible : générer une erreur `no_source_avaible`

## Tool check_document_applicability : vérification que les sources documentaires s'appliquent à une date donnée
### 1. objectif
Parmi les contraintes de l'audit, nous avons "La validité temporelle des textes et des décisions doit être prise en compte."
Nous devons donc vérifier si un règlement s'applique à une date donnée.

Le tool doit renvoyer le résultat factuel d'une recherche d'un règlement à une date donnée   `check_date` (`applicable` / `not_applicable`) : <span style="color:red">il ne doit pas interpréter, il ne doit pas inventer. </span> S'il ne sait pas, il doit renvoyer `needs_review`.

Il n'a pas non plus vocation à donner une conclusion d'audit.

**En résumé :**
- Si un règlement est applicable à la date demandée, renvoyer `applicable`
- Si un règlement n'est pas applicable à la date demandée, renvoyer `not_applicable` (ex : Si `check_date` est antérieure à `effective_date` de la régulation, ex : Si un règlement a été revu/abrogé (`revision_date` renseignée dans le fichier `donnees/donnees_tabulaire/regulations.csv`) et que `check_date` est postérieure à cette date, il doit renvoyer le status `not_applicable`, avec la `revision_date` pour permettre une explication de la raison du status.

### 2. entrées
- check_date : date à laquelle, on souhaite vérifier l'applicabilité d'un règlement. Par défaut prendre la date du jour
- regulation_id: la référence d'un règlement.
### 3. sorties
- status : "applicable" ou "not_applicable" du règlement, "needs_review" s'il y a ambiguité et nécessite une approbation humaine
- source {regulation, start_date} : 
    - regulation : le document référençant la reference_id permettant de controler le status. 
    - start_date : la date d'entrée en vigueur d'un règlement

### 4. permissions
- Lecture du fichier `donnees/donnees_tabulaire/regulations.csv`.
- Pas d'écriture
- Pas d'appel d'api externe

### 5. erreurs possibles
- Si `regulation_id` est vide : générer une erreur `invalid_input`
- Si la `regulation_id` n'existe pas dans `regulations.csv` : générer une erreur `no_reference` car le tool ne doit rien inventer.
- Si `check_date` est dans un format invalide : générer une erreur `invalid_date`.
- Si les données source sont ambigues ou incohérentes (ex : deux versions d'un même règlement avec des dates qui se chevauchent) : générer une erreur `ambiguous_reference` (ou remonter status = `needs_review` avec source contenant les références aux multiples sources)

## Tool search_precedents : recherche de précédents / de décisions de référence
### 1. objectif
Extraire des décisions précédents ayant fait "jurisprudence" qui ont donné lieu à des sanctions.

Le tool ne doit renvoyer que des décisions de précédence à partir de critère de recherche fourni en input (mots clés ou phrases).
L'objectif est de récupérer des cas antérieurs qui ont donné lieu à une décision `not_compliant`.

Le tool ne doit pas décider si les éléments fournis en input sont `compliant` ou `not_compliant`

**En résumé :**
- Si des décisions correspondent à des mots clés ou phrase à rechercher, renvoyer la liste des décisions trouvées dans le fichier `donnees/sources_documentaires/decisions_reference.md` et faire le matching de la règlementation avec `donnees/donnees_tabulaire/regulations.csv`
- Si aucune décision ne correspond, renvoyer une liste vide

<span style="color:red">Le tool ne doit pas :
- choisir "la meilleure" décision de lui-même. Il doit renvoyer la liste des décisions
- prendre de décision quant à la compliance ou non 
- retrouver l'entité qui a eu des sanctions en allant regarder `controls.csv` ou `non_conformiies.csv`
</span>

### 2. entrées
- request_input : mots clés ou phrase à rechercher

### 3. sorties
- decision : liste de décisions avec {date, entity, summary, regulation_id, sanction}
    - date : date de rendu de la décision
    - entity : champs entité de `decisions_reference.md`
    - summary : explique et résume le motif de la decision de sanction
    - regulation_id : lien vers la régulation (référence qui renvoie vers l'article ou le règlement applicable)
    - sanction : decision de sanction
- si aucune decision ne correspond aux critères de recherche, renvoyer une liste vide

### 4. permissions
- Lecture du fichier ``donnees/donnees_tabulaire/regulations.csv` et `donnees/sources_documentaires/decisions_reference.md`.
- Pas de lecture des fichiers `donnees/donnees_tabulaire/controls.csv` ou `donnees/donnees_tabulaire/non_conformities.csv`
- Pas d'écriture
- Pas d'appel d'api externe

### 5. erreurs possibles
- Si `request_text` est vide : générer une erreur `invalid_input`.
- Si le fichier de référence n'existe pas / accessible : générer une erreur `no_source_avaible`

