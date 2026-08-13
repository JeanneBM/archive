gh repo delete JeanneBM/. --yes

```
#!/usr/bin/env bash
set -euo pipefail

repo="${1:?Użycie: ./archive-repo.sh NAZWA_REPO}"

if [[ ! -d "archive/.git" ]]; then
    echo "Błąd: katalog archive nie jest repozytorium Git."
    exit 1
fi

if [[ -e "archive/$repo" ]]; then
    echo "Błąd: archive/$repo już istnieje."
    exit 1
fi

git clone "https://github.com/JeanneBM/${repo}.git"

cd "$repo"

find . -name ".git" -type d -prune -exec rm -rf -- {} +

cd ..

mv -- "$repo" archive/

cd archive

git add .
git commit -m "add $repo"
git push origin

cd ..

echo "Gotowe: $repo został dodany do archive i wysłany na GitHub."
```
