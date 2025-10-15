Stratégie de Branches (GitFlow Minimal)
Ce document définit la stratégie Git adoptée par le projet, inspirée de GitFlow, dans une version simplifiée et pragmatique.
L’objectif est de garantir un développement propre, des versions stables et un historique Git clair.

🌳 Branches Principales
Branche	Rôle	Remarques
main	Contient uniquement le code en production. Chaque commit sur cette branche correspond à une version stable (taguée).	Branche protégée, fusion via PR uniquement.
develop	Contient le code de développement intégré. Toutes les nouvelles fonctionnalités y sont fusionnées une fois terminées et testées.	Sert de base pour les branches feature et release.
🌿 Branches Secondaires
Type	Rôle	Part de…	Fusionne vers…	Exemple
feature/	Développement d’une nouvelle fonctionnalité	develop	develop	feature/login-page
release/	Préparation d’une version avant mise en production	develop	main et develop	release/v1.2.0
hotfix/	Correction d’un bug critique en production	main	main et develop	hotfix/v1.2.1
⚙️ Flux de Travail
🧩 1. Développement d’une fonctionnalité (feature)
Créer la branche à partir de develop :
git checkout -b feature/nom-de-la-feature develop
Développer et committer avec des messages clairs (feat:, fix:, refactor:…).
Se synchroniser régulièrement :
git pull --rebase origin develop
Ouvrir une Pull Request vers develop.
Une fois validée et testée, la PR est fusionnée avec “Squash and Merge”.
🚀 2. Préparation d’une version (release)
Créer la branche de release :
git checkout -b release/v1.2.0 develop
Effectuer les derniers ajustements : documentation, corrections mineures, version bump.
Ouvrir une PR vers main.
Après validation et fusion :
git checkout main
git pull
git tag v1.2.0
git push origin main --tags
Rétro-fusionner vers develop pour conserver la cohérence :
git checkout develop
git merge --no-ff main
git push origin develop
🩹 3. Correction urgente (hotfix)
Créer une branche depuis main :
git checkout -b hotfix/v1.2.1 main
Apporter le correctif.
Ouvrir une PR vers main.
Une fois validée :
git checkout main
git merge --no-ff hotfix/v1.2.1
git tag v1.2.1
git push origin main --tags
Rétro-fusionner sur develop :
git checkout develop
git merge --no-ff hotfix/v1.2.1
git push origin develop
🧱 Règles de Pull Request et CI
✅ Fusion par “Squash and Merge” uniquement
👀 Minimum un reviewer obligatoire avant merge
🧪 Tous les tests CI (lint, build, tests) doivent réussir
🚫 Aucun commit direct sur main ou develop
🧩 Commits conventionnels recommandés :
feat: ajout d’une nouvelle fonctionnalité
fix: correction d’un bug
docs: mise à jour documentation
🏷️ Versioning (Semantic Versioning)
Le projet suit la convention SemVer :

vMAJEUR.MINEUR.PATCH
Type	Exemple	Signification
MAJEUR	v2.0.0	Changements incompatibles (rupture d’API)
MINEUR	v1.1.0	Nouvelles fonctionnalités rétro-compatibles
PATCH	v1.0.1	Corrections de bugs ou hotfix
Créer et pousser un tag :

git tag v1.1.0
git push origin v1.1.0
🔄 Processus de Back-Merge
Après chaque release ou hotfix, toujours réintégrer main dans develop :

git checkout develop
git merge main
git push origin develop
Cela garantit que develop contient toutes les mises à jour de production.