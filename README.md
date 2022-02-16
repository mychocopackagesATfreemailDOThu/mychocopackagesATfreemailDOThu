# Hogyan publikálj csomagot Chocolatey-re?

```powershell
# új projekt létrehozása
choco new PROJEKTNEV

# projekt mappájának megnyitása
cd PROJEKTNEV
```

PROJEKTNEV.nuspec fájlban minden szükséges kitöltése. Adatok a szoftver honlapjáról kereshetők ki. Pár megjegyzés:

- az `owner` részre írd be a saját chocolatey felhasználóneved
- a `maintainer` legyen a szoftver készítőjének neve
- az `iconUrl` nem lehet `https://raw.github...` kezdető, tehát nem hosztolódhat a githubon, helyette például: https://statically.io/
- hozz létre a csomag forrásának egy új repot githubon és helyezd a `packageSourceUrl` részre a linkjét

## chocolatelyinstall.ps1

- `$url` kitöltése
- `filetype` kitöltése (exe)
- `checksum` kitöltése, generálás:
    - `choco install checksum`
    - `checksum -t sha256 -f VALAMI.EXE`
- Alul semmi más ne legyen aktív. Példákat láthatsz az eddig feltöltött csomagoknál: https://github.com/mychocopackagesATfreemailDOThu
- Távolítsuk el az összes kommentet a fájlból

Install tesztelése:

```powershell
choco pack
choco install PROJEKTNEV.nupkg -dv -s . --force -y
```

## chocolatelyuninstall.ps1

- `filetype` kitöltése
- Alul semmi más ne legyen aktív, csak töröljük le (Powershell kóddal -portable alkalmazások esetén - vagy beépített Chocolatey paranccsal vonjuk vissza a telepítést). Példákat láthatsz az eddig feltöltött csomagoknál: https://github.com/mychocopackagesATfreemailDOThu

Unnstall tesztelése:

```powershell
choco pack
choco uninstall PROJEKTNEV.nupkg -dv -s . --force -y
```

## Push

1. https://community.chocolatey.org/account/Register
2. https://community.chocolatey.org/account/  api key szekcióban látod az api kulcsod.

```powershell
# apikulcs regisztrálás
choco apikey --key APIKULCS --source https://push.chocolatey.org/

# feltöltés
choco push PROJEKTNEV.nupkg --source https://push.chocolatey.org/
```

Push után küld emailt, abban benne lesz a csomag oldalának linkje. Egy bot elvégez ellenőrzéseket, pl vírus és telepítés ellenőrzés. Ezekről a package oldalán egy chatablak szerű szekcióban jelennek meg információk. A végén a bot ad "recommendation"-öket, amiket érdemes elvégezni, de ha nem szól az emberi review-er akkor nem kötelező. Pár nap múlva, kb 7-10 nap ember reviewer is válaszol és engedélyezi a package-t. 

## Git push

A csomag forráskódját tárold githubon (amit megjelöltél a nuspec fájlban `packageSourceUrl` helyen)

```bash
# navigálj a projekt mappájába, majd ez mindent megcsinál:
git config --local credential.helper ''; git config user.name mychocopackagesATfreemailDOThu; git config user.email mychocopackages@freemail.hu; git add .; git commit; git push;
```
