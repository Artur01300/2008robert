# CAXA Lathe 2008 - Diagnostic

This repository only contains the Windows launcher (`LatheN.exe`) and a few
auxiliary files that were extracted from an installation of **CAXA Lathe 2008**.
When the executable is started on a fresh machine it immediately exits with the
message:

> Impossible d'exécuter le code, car customui.dll est introuvable.

The crash occurs before any custom logic can run because the executable expects
`customui.dll` to be available in the same installation folder. The binary has a
hard dependency on many exported functions from that library (for example,
`CBCGToolbarCustomize::OnInitDialog`, `CBCGToolBar::m_bAltCustomization`, etc.).
Without the original library the process aborts during dynamic linking.

Unfortunately the original `customui.dll` is not present anywhere in the
repository. Re-compiling the library is not possible because the source code is
not provided and the import list shows hundreds of proprietary MFC classes that
cannot be stubbed realistically.

## Résolution proposée

1. Récupérer l'installateur officiel de **CAXA Lathe 2008** depuis un support
   authentique (CD ou image ISO) ou via l'éditeur si une version de maintenance
   est toujours disponible.
2. Lancer l'installation complète pour que tous les composants nécessaires –
   dont `customui.dll` – soient copiés dans `C:\Program Files\CAXA\CAXALATHE\bin`.
3. Copier le contenu de ce dossier sur la machine où vous souhaitez exécuter le
   logiciel. Assurez-vous que `customui.dll`, `LatheN.exe` et le reste des DLL
   propriétaires restent ensemble dans le même répertoire.
4. Éviter de déplacer uniquement l'exécutable : les bibliothèques dynamiques MFC
   et les thèmes (`customui.dll`, `BCGCBPRO###.dll`, etc.) sont indispensables au
   démarrage.

## Pourquoi une réécriture n'est pas réaliste

Une réécriture complète demanderait l'accès au code source et à la documentation
fonctionnelle du logiciel métier. Sans ces éléments, il est impossible de
reproduire l'ensemble des fonctionnalités de FAO offertes par CAXA Lathe 2008.

Si votre objectif est de prolonger la durée de vie du logiciel sur des systèmes
modernes, la meilleure approche consiste à contacter l'éditeur pour obtenir une
version à jour ou un patch officiel. Toute tentative de recréer `customui.dll`
à partir de zéro violerait probablement les licences et serait très risquée en
termes de stabilité.

## Journal de diagnostic

- 2023-??-?? – Analyse du binaire `LatheN.exe` avec `objdump` : dépendances
  dynamiques sur `customui.dll` confirmées.

