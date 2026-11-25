# radar.namstudio.it

Un dominio, più ambienti. Ogni cliente che non ha un sottodominio suo entra da qui con la sua password.

## Come funziona

- `index.html` chiede la password, calcola `sha256("nam-radar:" + password)` e prende i primi 16 caratteri: quella è la cartella dell'ambiente.
- Se la cartella esiste, la password viene passata alla pagina dell'ambiente (sessionStorage) e la pagina si apre. Se non esiste: "password non riconosciuta". Nessun elenco degli ambienti è pubblicato.
- La pagina di ogni ambiente è cifrata con la stessa password (`proteggi.py` del motore radar, AES-GCM). Media e video restano in chiaro, con il nome dell'id inserzione.

## Ambienti

| Cliente | Cartella | Workspace |
|---|---|---|
| Sonia (IPNOSAN + Scuola M.A.G.I.A.) | `25dedec0057cce5f/` | `~/radar-sonia` |

## Aggiungere un ambiente

```bash
H=$(python3 -c "import hashlib;print(hashlib.sha256(b'nam-radar:PASSWORD').hexdigest()[:16])")
# nel workspace del cliente: cliente.dominio = "radar.namstudio.it/$H" in competitors.json
python3 tools/build_site.py
RADAR_PASSWORD=PASSWORD /usr/bin/python3 tools/proteggi.py
mkdir -p ~/radar-hub/$H && cp -R site/. ~/radar-hub/$H/
```

Poi commit e push (i video a blocchi, la rete cade sui push grossi).
