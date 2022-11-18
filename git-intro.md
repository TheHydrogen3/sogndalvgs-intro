# Git-oppgåver
Denne artikkelen inneheld grunnleggande operasjonar ifht. Git.
## A) Opprett lokalt repository og legg til ei fil
1. Lag ei mappe som skal verta til eit Git-repository
2. Gå inn i mappa i terminalen: `cd <sti-til-mappa>`
3. Opprett eit repository i mappa `git init`
4. Sjekk status på repositoriet ditt: `git status`
5. Opprett ei tekstfil i mappa. Namn og innhald bestemmer du sjølv, men viss du treng idear: Lag ei fil som heiter hello.txt, med innhaldet "Hello Git".
6. Marker fila for versjonskontroll: `git add <namn-på-fila>`.
7. Sjekk status for repositoriet igjen. Output skal vera noko i denne dur:
```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   hello.txt
```
8. Legg den valde fila inn i versjonskontroll: `git commit`
9. Du får no opp ein tekst-editor der du skal legga inn "commit-meldinga", som er ei beskriving av endringa. Her kan det vera behov for tips, og då må du berre spørja oss.
10. Når committen er vellykka, vert liknande linjer skrivne ut:
```
$ git commit
[main (root-commit) 69db5bf] Initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```
11. Ein ny status-sjekk vil no rapportera dette:
```
$ git status
On branch main
nothing to commit, working tree clean
```
12. Gjer ei endring i fila.
13. Sjekk status igjen. Legg merke til at Git kjem med forslag til neste steg.
```
$ git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
14. Legg til endringane i fila, og gjer ein ny commit.
15. Sjekk loggen til repositoriet: `git log`. Du skal no sjå dei to commit-ane dine, med ID, forfattar, tidsstempel og commit-melding.
```
$ git log
commit 23dd67093995d5625531dbac572eedf4d5e7a960 (HEAD -> main)
Author: Johannes Molland <12180816+johannmo@users.noreply.github.com>
Date:   Thu Nov 17 21:31:03 2022 +0100

    endring

commit 69db5bf9760c31f04204616b5f756783e56924bf
Author: Johannes Molland <12180816+johannmo@users.noreply.github.com>
Date:   Thu Nov 17 21:23:07 2022 +0100

    Initial commit
```
16. Du finn ut at endringa di var feil, og vil fjerna den siste commit-en din. Det kan du oppnå vha. `git revert <commit-id>`:
```
$ git revert 23dd67093995d5625531dbac572eedf4d5e7a960
[main fc7e795] Revert "endring"
 1 file changed, 1 insertion(+), 1 deletion(-)
```
17. Sjekk Git-loggen til repositoriet ditt ein gong til, og legg merke til at Git har lagt til ei ny commit som inneheld omvende endringar frå den du ville fjerna. Det verkar som om commit-en du reverterte er fjerna, men i verkelegheita er det lagt til ein ny commit som nullar ut endringa.
```
$ git log
commit fc7e7951530a5468375e4e5f0ee8b692935fd520 (HEAD -> main)
Author: Johannes Molland <12180816+johannmo@users.noreply.github.com>
Date:   Thu Nov 17 21:35:45 2022 +0100

    Revert "endring"

    This reverts commit 23dd67093995d5625531dbac572eedf4d5e7a960.

commit 23dd67093995d5625531dbac572eedf4d5e7a960
Author: Johannes Molland <12180816+johannmo@users.noreply.github.com>
Date:   Thu Nov 17 21:31:03 2022 +0100

    endring

commit 69db5bf9760c31f04204616b5f756783e56924bf
Author: Johannes Molland <12180816+johannmo@users.noreply.github.com>
Date:   Thu Nov 17 21:23:07 2022 +0100

    Initial commit
```
---
## B) Opprett ein branch (grein), og legg til ei utviding av repositoriet
Når me gjer ei utviding av kodebasen (ny funksjonalitet eller feilretting), gjer me dette som oftast i form av ein ny branch. Då får me utvikla det me jobbar med for seg sjølv, og slepp å forhalda oss til andre endringar som kan komma inn frå andre utviklarar.
1. Opprett ein branch i repositoriet ditt: `git checkout -b skriv-eit-namn-på-branchen-her`
```
$ git co -b my-branch
Switched to a new branch 'my-branch'

jom@LU-JOM-T490s-2 MINGW64 /c/temp/repo (my-branch)
```
Legg merke til at branchen du jobbar mot har endra seg til den du akkurat laga. Endringane du gjer vil ikkje verta med i "main"-branchen før du flettar dei inn.
2. Legg til ei ny fil i repositoriet (add og commit).
3. Gjer ein ny sjekk Git-loggen, og sjå at alle commit-ane er på plass i output. Loggen viser også kva branch-ar som har dei ulike commit-ane.
```
$ git log
commit 72081f173a1afb1b4c7728cb5ff27506b85fc116 (HEAD -> my-branch)
Author: Johannes Molland <12180816+johannmo@users.noreply.github.com>
Date:   Thu Nov 17 21:47:28 2022 +0100

    fil2

commit fc7e7951530a5468375e4e5f0ee8b692935fd520 (main)
Author: Johannes Molland <12180816+johannmo@users.noreply.github.com>
Date:   Thu Nov 17 21:35:45 2022 +0100

    Revert "endring"

    This reverts commit 23dd67093995d5625531dbac572eedf4d5e7a960.

commit 23dd67093995d5625531dbac572eedf4d5e7a960
Author: Johannes Molland <12180816+johannmo@users.noreply.github.com>
Date:   Thu Nov 17 21:31:03 2022 +0100

    endring

commit 69db5bf9760c31f04204616b5f756783e56924bf
Author: Johannes Molland <12180816+johannmo@users.noreply.github.com>
Date:   Thu Nov 17 21:23:07 2022 +0100

    Initial commit
```
4. Du er no fornøgd med utviklinga i branchen, og ønsker å fletta inn endringane i hovud-greina (main). Skift først attende til "main"-branchen vha. `git checkout main`
5. Flett inn endringane frå den andre branchen i "main": `git merge skriv-namnet-på-den-andre-branchen-her`. Dersom operasjonen er vellykka, ser kommandolinja liknande ut:
```
$ git merge my-branch
Updating fc7e795..72081f1
Fast-forward
 fil2.txt | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 fil2.txt
```
6. Sjekk loggen til repositoriet ein gong til, og legg merke til at branch-namna har flytta litt på seg ifht. commit-ane i loggen.
7. Du kan no sletta den andre branchen vha. `git branch -d skriv-namnet-på-den-andre-branchen-her`
8. Dersom du sjekkar loggen igjen, ser du at branchen du sletta ikkje lenger er med i loggen.
