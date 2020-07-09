# Git CheatSheet

## Table of contents
- [Git CheatSheet](#git-cheatsheet)
  - [Table of contents](#table-of-contents)
  - [Usuwanie plików, które nie trafiły do repo z kolejki](#usuwanie-plików-które-nie-trafiły-do-repo-z-kolejki)
  - [Odwrotność git add](#odwrotność-git-add)
  - [Usuwanie oraz przenoszenie plików w repozytorium](#usuwanie-oraz-przenoszenie-plików-w-repozytorium)
  - [Przywrocanie stanu pliku lub całego projektu z repo](#przywrocanie-stanu-pliku-lub-całego-projektu-z-repo)
      - [Przywracanie zmian commitu](#przywracanie-zmian-commitu)
  - [Przeglądanie historii](#przeglądanie-historii)
  - [Jak pisać komentarze?](#jak-pisać-komentarze)
  - [Dodawanie na stos](#dodawanie-na-stos)
  - [Branch](#branch)
  - [Fork](#fork)
  - [Rozwiązawanie konfliktów](#rozwiązawanie-konfliktów)
  - [Rebase](#rebase)
  - [Tagi](#tagi)

## Usuwanie plików, które nie trafiły do repo z kolejki

```bash
    git clean <-n,-d,-i etc>
```

---

## Odwrotność git add

```bash
    git reset
    git reset <hash> # usuwa commit i przywraca zmiany
```

---

## Usuwanie oraz przenoszenie plików w repozytorium

```bash
    git rm / mv
```

---

## Przywrocanie stanu pliku lub całego projektu z repo

```bash
    git checkout <path> # <- przywraca plik
    git checkout <githash> # <- przywraa projekt z commitu o podanych hashu
    git checkout <githash> <path> # przywraca stan pliku z podanego commitu
```

---

#### Przywracanie zmian commitu

Bierzemy commit poprzedni i odwracamy go i dajemy jako nowy. Nie zmienia to stanu poprzednich commit'ów

```bash
    git revert <hash>
    git revert <mergahash> -m 1 # odwraca commit z mergem
```

---

## Przeglądanie historii

Poszczególne opcje można ze sobą łączyć

```bash
    # hashe i komentarze
    git log --oneline -liczba commitów
    # comity zmieniające dany folder
    git log --oneline -- js/
    # wyświetlenie zmian
    git log --oneline --patch -- js/
    # commity tylko jednego autora
    git log --author="Marcin"
    # szukanie po komentaezu commita
    git log --grep="fraza"
```

---

## Jak pisać komentarze?

1. Podziel komentarz na tytuł oraz treść
   - aby stworzyć tytuł musisz zapewnić przerwę (pusty wiersz) między tytułem a komentarzem
   - aby to uzyskać nie można użyc -m
1. Rozpocznij dużą literą i nie końćz kropką
1. Użyj trybu rozkazującego w tytule np: "Add index.html"
1. Powinien odpowiadać na pytanie co i dlaczego

---

## Dodawanie na stos

Zmodyfikowałeś brancha a musisz się przełączyć? Aby nie tracić danych odłóż je na stos.

```bash
    git stash # odkładamy na stos
    git checkout branch # przełączmy
    git stash pop # przywracamy zmiany ze stosu
```

Jeśli zmian będzie dużo przydatne będzie

```bash
    # dodaje komenta do stosu
    git stash push -m "komentarz"
    # wyświetla dane ze stosu
    git stash list
    # przywracanie wybranych zmian
    git stash pop <identyfikator>
    # usuwanie zmian na stosie
    git stash drop <identyfikator>
    git stash clear
```

---

## Branch

Stworzenie i przełącznie się między branchami

```bash
    git branch feature
    git checkout feature
```

Merge feature do master:

- musisz być na master

```bash
    git merge feature
    git branch -D feature # usuwa branch
```

**Podział repozytorium**

- _Master_
  - _Dev_ - wersja testowa projektu
    - _Feature_ - nowe funckje
      - _User Branch_ - gałąź użytkownika
      - _Test / Bugfix_ - do testowania/łatania błędów

Pobranie zmian z repozytorium

```bash
    git fetch
    git merge origin master
```

---

## Fork

1. Najpierw zrób fork na zdalnych repo, potem pobierz repo na swój komputer przez git clone.
1. Potem robisz zmiany wysyłasz na swoje repo.
1. Wysyłasz pull request przez githuba do właściciela oryginalnego brancha, aby przyjął twoje zmiany.
1. Usuwasz zmiany i zdalny branch
1. Dodajesz oryginalne repo jak zdalne
   ```bash
       git remote add upstream <adres repo>
   ```
1. Pobieranie zmian ze zdalnego repo
   ```bash
       git fetch upstream
   ```
1. Gdy właściel zaakcpetuje nasz pull request nasz branch przestanie być aktualny. Gdy tak się stanie musimy zaaktualizować nasz branch master do master z upstream
   ```bash
       git merge upstream/master
       git push
   ```

**Pobranie zmian po pull request na naszym branchu**

```bash
    git pull
```

---

## Rozwiązawanie konfliktów

Robisz sobie git merge, a git nagle wypluwa konflikt

```bash
   CONFLICT (content): Merge confilict in index.html
```

Otwierasz plik

```txt
<<<<<<< master
    <h1>Hello world!</h1> <-- wesja z brancha naszego -->
=======
    <h2>Hello world!</h2>
>>>>>> feature
```

Usuwasz górę albo dół

Możliwy jest konflikt gdy plik zostanie usunięty. Wtedy robisz jedną z rzeczy:

- git add plik
- git rm plik

---

## Rebase

Kopiuje zmiany z commitów z brancha feauture i wkleja je jako najnowsze do mastera niezapominając o zmianach zrobionych w masterze. Sprawia, że historia pokazuje, że branch feature powstał na podstawie najnowszego commita branchu master, chociaż wcale nie musiało tak być.

Z brancha feature wpisujemy polecenie:

```bash
    git rebase master
```

Następnie przesuwamy wskaźnik mastera na najnowczy commit

```bash
    git checkout master
    git merge feature
```

Gdy dojdzie do konfilktu możemy:

- rozwiązać konflikt i wpisać
  ```bash
      git rebase --continue
  ```
- anulować rebase:
  ```bash
      git rebase --abort
  ```

---

## Tagi

Używamy do oznaczania ważnych commitów ( np. w danych commicie skończyliśmy daną wersje projektu )

Wypchneliśmy dany commit wtedy:

```bash
    git tag v1.0 -a -m "version 1.0"
    gut push --tags
```

Usuwanie tagów

```bash
    git tag -d v1.0 # <- usuwa tag z lokalnego repo
    git push origin -d v1.0 # <- usuwa tag ze zdalnego repo
```
