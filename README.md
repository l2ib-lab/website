# www.l2ib.fr — site de l'équipe L2IB

Site vitrine de l'équipe **L2IB (Leukemia and Lymphoma Immune Biology)**, CIRI, Lyon.
Construit avec [Quarto](https://quarto.org), publié automatiquement sur GitHub Pages.

---

## 1. Prérequis (une seule fois)

Installez [Quarto](https://quarto.org/docs/get-started/). Aucun R ni Python
n'est nécessaire : le site est en Markdown pur.

Vérifiez :

```bash
quarto --version
```

## 2. Travailler en local

```bash
quarto preview
```

Ouvre le site dans le navigateur et le recharge à chaque sauvegarde.
`Ctrl+C` pour arrêter.

Pour construire sans prévisualiser :

```bash
quarto render
```

Le résultat est dans `_site/` (ignoré par git — c'est GitHub qui le construit).

## 3. Mise en ligne (une seule fois)

1. Créez l'organisation GitHub **`l2ib-lab`**, puis le dépôt **public**
   `website`.

   > Le nom `l2ib` seul était déjà pris sur GitHub. Sans conséquence :
   > le nom de l'organisation n'apparaît que dans l'URL des dépôts et dans la
   > ligne DNS ci-dessous — jamais pour les visiteurs, qui arrivent sur
   > `www.l2ib.fr`.
   >
   > Le dépôt peut porter **n'importe quel nom** dès lors qu'on lui attache un
   > domaine personnalisé (étape 4) : il est alors servi à la racine de ce
   > domaine. Le nom `<organisation>.github.io` n'est imposé que si l'on se
   > passe de domaine personnalisé, auquel cas le site vivrait sous
   > `l2ib-lab.github.io/website/`.
   >
   > Attention : la cible DNS de l'étape 5 reste `l2ib-lab.github.io.`
   > quel que soit le nom du dépôt. C'est l'adresse des serveurs GitHub Pages
   > de l'organisation, pas celle du dépôt.
2. Poussez ce dossier :

   ```bash
   git init
   git add .
   git commit -m "Initial site skeleton"
   git branch -M main
   git remote add origin https://github.com/l2ib-lab/website.git
   git push -u origin main
   ```

3. Sur GitHub : **Settings → Pages → Build and deployment → Source =
   `GitHub Actions`**.
   Le workflow `.github/workflows/publish.yml` fait le reste.
4. **Settings → Pages → Custom domain** : saisissez `www.l2ib.fr`,
   puis cochez **Enforce HTTPS** (attendez que le certificat soit émis,
   quelques minutes à quelques heures).
5. Chez votre registrar (OVH…), dans la zone DNS de `l2ib.fr` :

   ```text
   www    CNAME   l2ib-lab.github.io.

   @      A       185.199.108.153
   @      A       185.199.109.153
   @      A       185.199.110.153
   @      A       185.199.111.153
   ```

   Les quatre enregistrements `A` font pointer `l2ib.fr` (sans www) vers
   GitHub, qui redirige alors vers `www.l2ib.fr`.

Ensuite, **chaque `git push` sur `main` republie le site** (1 à 2 minutes).

## 4. Structure

```text
_quarto.yml              Configuration : navbar, footer, thème, métadonnées
styles.scss              Couleurs et styles (3 variables en haut du fichier)
index.qmd                Accueil
research.qmd             Axes de recherche
tools/index.qmd          Liste des applications Shiny
tools/fibom-ai/index.qmd URL stable + redirection vers l'app FIBOM-AI
publications.qmd         Généré depuis references.bib
team.qmd                 Membres
join.qmd                 Offres M2 / thèse / post-doc
legal.qmd                Mentions légales (obligatoire)
references.bib           Export Zotero
images/                  Logo, favicon, photos, figures
.github/workflows/       Build + déploiement automatiques
CNAME                    Nom de domaine (www.l2ib.fr)
```

## 5. Tâches courantes

| Je veux… | Fichier à modifier |
|---|---|
| Changer les couleurs | les 3 variables en haut de `styles.scss` |
| Ajouter une entrée au menu | `_quarto.yml` → `navbar: left:` |
| Mettre à jour les publications | écraser `references.bib` (export Zotero) |
| Ajouter un membre | `team.qmd` + photo dans `images/team/` |
| **Ajouter une app Shiny** | voir ci-dessous |
| **Changer l'hébergeur d'une app** | uniquement `tools/<app>/index.qmd` |

### Ajouter une application Shiny

1. Déployez l'app sur [Posit Connect Cloud](https://connect.posit.cloud) depuis
   son dépôt GitHub (visibilité **Public**). Notez l'URL obtenue.
2. Créez l'URL stable :

   ```bash
   mkdir -p tools/mon-app
   cp tools/fibom-ai/index.qmd tools/mon-app/index.qmd
   ```

   Éditez ce fichier et remplacez l'URL cible aux deux endroits indiqués.
3. Dans `tools/index.qmd`, copiez le gabarit `.app-card` et remplissez-le.
4. `git push`.

L'app est alors citable de façon permanente à l'adresse
`https://www.l2ib.fr/tools/mon-app/`, quel que soit l'hébergeur réel.

## 6. À faire avant la mise en ligne publique

- [ ] Compléter les champs `[…]` de `legal.qmd` (directeur de publication, DPO)
      et faire valider par la tutelle
- [ ] Remplacer tous les blocs `::: {.todo}` (encadrés rouges) puis les supprimer
- [ ] Remplacer `images/logo.svg` et `images/favicon.svg`
- [ ] Remplacer `references.bib` par l'export Zotero réel
- [ ] Créer l'adresse `contact@l2ib.fr` (ou remplacer partout par une adresse existante)
- [ ] Vérifier l'adresse postale exacte dans `index.qmd` et `legal.qmd`
- [ ] Demander au CIRI d'ajouter le lien vers `www.l2ib.fr` sur
      [la page de l'équipe](https://ciri.ens-lyon.fr/teams/lib)
- [ ] Migrer FIBOM-AI de shinyapps.io vers Posit Connect Cloud
      (shinyapps.io ferme aux nouvelles apps fin 2026)

## Licence

Contenu : [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
Code du site : MIT.
