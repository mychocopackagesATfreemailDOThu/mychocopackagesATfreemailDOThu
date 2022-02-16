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
- 

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
