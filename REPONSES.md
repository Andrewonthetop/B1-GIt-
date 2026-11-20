# TP Versionning et Git — Réponses

**Nolan Tellier** — Cours de Tom Paya

---

## Exercice 1 — Prise en main complète de Git (cours 01)

### Questions des étapes

- **Étape 5 :** après `git checkout main` puis `git merge feature/contact`, la commande `ls` montre bien `contact.html` sur `main`.
- **Étape 6 :** `test.log` n'apparaît pas dans `git status` parce que la règle `*.log` du `.gitignore` l'ignore. Seul `.gitignore` est proposé au suivi.

### Questions finales

**1. Quelle différence existe-t-il entre `git add` et `git commit` ?**
`git add` place des modifications dans la zone de staging : on prépare ce qui partira dans le prochain commit. `git commit` enregistre définitivement ces modifications dans l'historique du dépôt, avec un message.

**2. À quoi sert la zone de staging ?**
Elle sert à choisir précisément les modifications qui feront partie du prochain commit. On peut ainsi faire des commits propres, chacun correspondant à une étape logique, même si on a modifié plusieurs fichiers en même temps.

**3. Pourquoi travailler sur une branche plutôt que directement sur `main` ?**
Une branche permet de développer une fonctionnalité ou une correction à part, sans casser la branche principale. `main` reste stable, et on ne fusionne le travail que lorsqu'il est terminé et vérifié. Ça permet aussi à plusieurs personnes de travailler en parallèle.

**4. À quoi sert une Merge Request ?**
Une Merge Request (sur GitLab) propose d'intégrer une branche dans une autre, en général dans `main`. Elle permet de présenter les modifications, de faire relire le code par l'équipe, d'en discuter, puis de valider et fusionner.

**5. Dans quelle situation un conflit peut-il apparaître ?**
Quand Git ne peut pas fusionner automatiquement deux modifications. Cas classique : deux branches ont modifié différemment la même ligne d'un même fichier. Il faut alors choisir la bonne version, supprimer les marqueurs `<<<<<<<`, `=======` et `>>>>>>>`, puis faire `git add` et `git commit`.

**6. À quoi sert le fichier `.gitignore` ?**
Il indique à Git les fichiers et dossiers à ne pas suivre : logs, fichiers temporaires, dépendances (`node_modules`), configuration propre à une machine ou fichiers sensibles comme `.env`. Attention, il n'arrête pas le suivi d'un fichier déjà commité.

**7. Pourquoi ne faut-il pas versionner un fichier `.env` contenant des identifiants ?**
Parce que tout ce qui est commité reste dans l'historique et part sur le dépôt distant. Les mots de passe, clés d'API ou identifiants de base de données seraient visibles par toute personne ayant accès au dépôt (voire par tout le monde si le dépôt est public). Même supprimés plus tard, ils resteraient dans l'historique.

**8. Quelle commande permet de connaître l'état actuel des fichiers du dépôt ?**
`git status`.

---

## Exercice 2 — Réparer un dépôt Git (cours 02)

### Questions des étapes

**Étape 1 — Quelle commande permet d'afficher toutes les branches dans le graphique ?**
`git log --oneline --graph --all` : c'est l'option `--all` qui affiche toutes les branches.

**Étape 2 — Commit examiné :** le commit « Ajout de la feuille de style » (identifiant affiché par `git log --oneline`), examiné avec `git show <hash>`.
Résumé : ce commit crée le fichier `style.css` (police Arial, marge de 40px, couleur du titre) et ajoute dans `index.html` la balise `<link>` qui relie la feuille de style.

**Étape 7 — Quelle option de reset avez-vous utilisée et pourquoi ?**
`git reset --soft HEAD~1`. Le commit de test est annulé, mais les modifications restent dans les fichiers et dans le staging : rien n'est perdu et on peut refaire directement un commit propre. (`--mixed` aurait aussi gardé les modifications mais en les retirant du staging ; `--hard` les aurait supprimées.)

**Étape 8 — Quelle différence observez-vous par rapport à reset ?**
Avec `revert`, le commit annulé reste visible dans l'historique et un **nouveau commit** « Revert … » est ajouté pour l'annuler. Avec `reset`, le commit avait complètement disparu de l'historique.

### Questions finales

**1. Quelle différence existe-t-il entre `git diff` et `git diff --staged` ?**
`git diff` montre les modifications qui ne sont **pas encore** ajoutées au staging. `git diff --staged` montre les modifications **déjà** ajoutées avec `git add`, c'est-à-dire ce qui partira dans le prochain commit.

**2. À quoi sert `git commit --amend` ?**
À corriger le dernier commit : changer son message (`git commit --amend -m "..."`) ou y ajouter un fichier oublié (`git add` puis `git commit --amend --no-edit`), sans créer de commit supplémentaire. Il réécrit le commit, donc son identifiant change.

**3. Quelle différence existe-t-il entre `git reset --soft` et `git reset --hard` ?**
Les deux annulent le commit. `--soft` garde les modifications dans les fichiers et dans le staging. `--hard` supprime tout : le commit, le staging et les modifications dans les fichiers.

**4. Pourquoi `git revert` est-il généralement préférable pour un commit déjà partagé ?**
Parce qu'il ne réécrit pas l'historique : il ajoute un nouveau commit qui annule l'ancien. Les autres développeurs qui ont déjà récupéré le commit ne se retrouvent pas avec un historique incohérent, et on garde une trace de l'annulation. `reset`, lui, supprime des commits que d'autres ont peut-être déjà.

**5. Dans quelle situation `git stash` est-il utile ?**
Quand on travaille sur quelque chose de pas terminé et qu'on doit changer de branche tout de suite (par exemple pour corriger un bug urgent), sans vouloir faire un commit incomplet. On met le travail de côté avec `git stash`, puis on le récupère avec `git stash pop`.

**6. Quelle commande permet de consulter les stash disponibles ?**
`git stash list`.

**7. Quelle commande utiliseriez-vous pour abandonner une modification locale non commitée ?**
`git restore fichier.txt` (et `git restore --staged fichier.txt` si le fichier avait déjà été ajouté au staging).

**8. Pourquoi faut-il être particulièrement prudent avec `git reset --hard` ?**
Parce qu'il est destructif : il supprime les modifications des fichiers, et le travail non commité est perdu définitivement, sans possibilité simple de le récupérer.
