# Sites — un dossier par établissement

Le frontend (design, composants, pages, images communes) est **unique** et partagé.
Chaque établissement n'a qu'un dossier de données ici.

```
sites/
└── epl-mon-avenir/
    └── site.json         les 5 champs du fichier Excel
```

## Les 5 champs (clés = en-têtes exacts de l'Excel)

```json
{
  "NOM PRENOMS": "GADJI Yebala",
  "PROFFESSION": "DIRECTEUR",
  "CONTACTS": "91880058/79448860",
  "ETABLISSEMENTS": "EPL MON AVENIR",
  "Quartier": "ASSIVIME (ANIE)"
}
```

Le gabarit `src/config/site-template.ts` en déduit tout le site : nom dans l'en-tête et le
pied de page, héros « Bienvenue à … », mot de la direction signé par le responsable,
bloc « Notre établissement », coordonnées, FAQ. **Rien n'est inventé** : ce qui n'est pas
dans l'Excel (email, horaires, slogan, tarifs, histoire) n'est simplement pas affiché
(tarifs : « Sur demande »).

## Ajouter un site

**Option A — à la main** : créer `sites/<id>/site.json` (id en minuscules, chiffres et
tirets, ex. `ecole-la-grace`) avec les 5 champs ci-dessus.

**Option B — depuis l'Excel** :
1. Excel → Fichier → Enregistrer sous → « CSV UTF-8 ».
2. `npm run site:nouveau -- chemin\vers\fichier.csv`
   Une ligne = un site. L'id est tiré du nom de l'établissement (`EPL MON AVENIR` → `epl-mon-avenir`).
   Les lignes incomplètes sont ignorées (signalées), un site existant n'est jamais écrasé
   (sauf `--remplacer`).

Puis :

```bash
npm run site -- liste                  # sites disponibles
npm run site -- epl-mon-avenir           # développement : http://127.0.0.1:5173
npm run site -- epl-mon-avenir build     # version à héberger : dist-sites/epl-mon-avenir/
```

Ou mettre `VITE_SITE=epl-mon-avenir` dans `.env.local`. Sans rien, c'est `epl-mon-avenir`
(build dans `dist/`).

## Isolation des données

Seul le dossier du site choisi est compilé (module virtuel `virtual:site` dans
`vite.config.ts`) : le build d'un établissement ne contient aucune donnée d'un autre.
Chaque site a aussi son propre stockage navigateur des données de démonstration et son
propre dossier de sortie.

## Plus tard : logo et photo du responsable

Tant qu'ils ne sont pas fournis, un monogramme (initiales) est affiché. Pour les ajouter :
placer les images dans `public/sites/<id>/` puis compléter `site.json` :

```json
  "logo": "/sites/epl-mon-avenir/logo.png",
  "photoResponsable": "/sites/epl-mon-avenir/fondateur.jpg"
```

## Textes communs à valider

Les cycles (Maternelle / Primaire / Collège), les pièces d'admission et les « valeurs »
viennent de `src/config/defaults.ts` et s'appliquent à tous les sites. Si un établissement
n'a pas de collège par exemple, il faudra pouvoir les surcharger : à signaler.
