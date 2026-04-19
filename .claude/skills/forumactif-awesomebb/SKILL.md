---
name: forumactif-awesomebb
description: >
  Skill expert pour maîtriser totalement Forumactif en version AwesomeBB. Utilise ce skill
  dès que l'utilisateur mentionne Forumactif, AwesomeBB, un forum, des templates de forum,
  la personnalisation CSS/JS d'un forum, une navbar de forum, un thème de forum, ou toute
  question sur la gestion/administration d'un forum Forumactif. Ce skill couvre l'intégralité
  de la gestion d'un forum AwesomeBB : administration, templates HTML, CSS, JavaScript,
  variables Forumactif, navbar custom, thèmes, mises à jour de la plateforme, et bonnes
  pratiques de design responsive. Déclenche ce skill même si l'utilisateur pose une question
  simple comme "comment changer ma navbar" ou "comment ajouter un lien dans mon menu".
---

# Skill : Maîtrise complète de Forumactif — Version AwesomeBB

## Vue d'ensemble

AwesomeBB est la version la plus récente et la plus moderne de Forumactif. Elle est **responsive par défaut** (s'adapte PC/mobile/tablette), et c'est la version que Forumactif utilise pour tester ses nouvelles fonctionnalités. Toute personnalisation passe par trois canaux : les **Templates HTML**, la **Feuille de style CSS**, et les **Scripts JavaScript**.

---

## 1. Panneau d'Administration (PA) — Structure

L'accès se fait via le menu du forum → **Connexion** → onglet **Administration**.

### Chemins essentiels

| Objectif                    | Chemin dans le PA                                              |
| --------------------------- | -------------------------------------------------------------- |
| Modifier les templates HTML | Affichage → Templates → Général                                |
| Modifier le CSS             | Affichage → Couleurs → Feuille de style CSS                    |
| Ajouter du JavaScript       | Affichage → Gestion des codes JavaScript                       |
| Liens de la navbar          | Affichage → Barre de navigation                                |
| Page d'accueil (portail)    | Page d'accueil → Généralités                                   |
| Widgets latéraux            | Affichage → Widgets                                            |
| Gérer les membres           | Membres → Gestion des membres                                  |
| Gérer les groupes           | Membres → Groupes d'utilisateurs                               |
| Modération                  | Modération → File d'attente                                    |
| SEO                         | Général → Configuration → SEO                                  |
| CSS additionnel             | Affichage → Couleurs → CSS additionnel _(ajouté en déc. 2025)_ |

> ⚠️ Les templates ne sont **visibles que par le compte Fondateur**. Les admins standards n'y ont pas accès.

---

## 2. Templates AwesomeBB — Liste et rôles

Les templates principaux se trouvent dans **Affichage → Templates → Général** :

| Template               | Rôle                                                               |
| ---------------------- | ------------------------------------------------------------------ |
| `overall_header`       | Structure globale du haut de page : navbar, logo, menu utilisateur |
| `overall_footer_begin` | Début du pied de page                                              |
| `overall_footer_end`   | Fin du pied de page, scripts de fermeture                          |
| `index_body`           | Page d'accueil : liste des catégories/forums                       |
| `index_box`            | Boîte individuelle d'une catégorie                                 |
| `viewforum_body`       | Affichage d'un forum (liste des sujets)                            |
| `viewtopic_body`       | Affichage d'un sujet (messages)                                    |
| `posting_body`         | Page de rédaction/réponse                                          |
| `profile_view_body`    | Profil d'un membre                                                 |
| `memberlist_body`      | Liste des membres                                                  |
| `search_body`          | Page de recherche                                                  |

> 📖 Pour une analyse détaillée de chaque template, consulte → `references/templates.md`

---

## 3. Structure HTML du `overall_header` (AwesomeBB)

C'est le template le plus modifié. En AwesomeBB, la navigation est générée **via JavaScript** et insérée dans des balises `<header>`.

### Structure simplifiée par défaut

```html
<body id="phpbb">
  <!-- Toolbar (barre top Forumactif) -->
  <div id="page-top">
    <header>
      <!-- Logo -->
      <a href="{U_INDEX}"><img src="{LOGO}" alt="{L_INDEX}" /></a>

      <!-- Navigation principale (JS-generated) -->
      <nav id="main-menu">...</nav>

      <!-- Menu utilisateur (JS-generated) -->
      <div id="user-menu">...</div>

      <!-- Notifications (JS-generated) -->
      <div id="notifications">...</div>
    </header>
  </div>
</body>
```

### Navbar custom avec liens

Pour créer une **navbar entièrement personnalisée**, remplace ou complète le `<nav>` dans `overall_header` :

```html
<nav id="custom-navbar">
  <ul>
    <li><a href="{U_INDEX}">Accueil</a></li>
    <li><a href="/forum/ma-categorie">Catégorie</a></li>
    <li><a href="{U_REGISTER}">S'inscrire</a></li>
    <li><a href="{U_LOGIN_LOGOUT}">Connexion</a></li>
  </ul>
</nav>
```

> 📖 Variables complètes disponibles → `references/variables.md`

---

## 4. Variables Forumactif essentielles

Les variables s'écrivent `{NOM_VARIABLE}` dans les templates.

### Variables de navigation

| Variable           | Résultat                                |
| ------------------ | --------------------------------------- |
| `{U_INDEX}`        | URL de l'accueil du forum               |
| `{U_REGISTER}`     | URL d'inscription                       |
| `{U_LOGIN_LOGOUT}` | URL connexion/déconnexion               |
| `{U_PROFILE}`      | URL du profil de l'utilisateur connecté |
| `{U_PRIVATEMAIL}`  | URL des messages privés                 |
| `{U_SEARCH}`       | URL de la recherche                     |

### Variables utilisateur (dans JS via `_userdata`)

En AwesomeBB, les variables `{USERNAME}` etc. **ne fonctionnent plus directement** dans les templates. Il faut les appeler via JavaScript :

```javascript
// Dans un script JS ou dans le template :
document.write(_userdata["username"]); // Pseudo
document.write(_userdata["avatar"]); // Avatar HTML
document.write(_userdata["rank"]); // Rang
document.write(_userdata["post_count"]); // Nb de messages
document.write(_userdata["registered"]); // Date d'inscription
```

Ou via une `<span>` + CSS :

```html
<span class="js-username"></span>
<script>
  $(function () {
    $.each(_userdata, function (key, value) {
      $(".js-" + key).html(value);
    });
  });
</script>
```

> 📖 Liste complète des variables → `references/variables.md`

---

## 5. CSS — Bonnes pratiques AwesomeBB

### Où écrire le CSS

- **Feuille de style principale** : Affichage → Couleurs → Feuille de style CSS  
  → Cocher **"Non"** à toutes les options CSS par défaut pour avoir le contrôle total.
- **CSS additionnel** _(nouveau, déc. 2025)_ : Affichage → Couleurs → CSS additionnel  
  → Idéal pour des ajouts ponctuels sans toucher au CSS principal.

### Classes CSS spécifiques AwesomeBB

#### États des catégories/sujets (AwesomeBB uniquement)

```css
.cat_no_new {
} /* Catégorie sans nouveau message */
.cat_new {
} /* Catégorie avec nouveau message */
.forum_no_new {
} /* Sous-forum sans nouveau message */
.forum_new {
} /* Sous-forum avec nouveau message */
.post_new {
} /* Sujet avec nouveau message */
.post_no_new {
} /* Sujet sans nouveau message */
.folder_locked {
} /* Sujet verrouillé */
.folder_sticky {
} /* Sujet de type note */
.folder_announce {
} /* Sujet de type annonce */
.folder_global {
} /* Annonce globale */
```

### Variables CSS (recommandé)

Déclare une palette globale en haut de ta feuille de style :

```css
:root {
  --primary: #3a86ff;
  --secondary: #ff006e;
  --bg: #1a1a2e;
  --text: #e0e0e0;
  --sm: 8px;
  --md: 16px;
  --lg: 32px;
}

/* Utilisation : */
.custom-navbar {
  background: var(--primary);
  padding: var(--md);
  color: var(--text);
}
```

### Responsive (AwesomeBB natif)

```css
/* Mobile : < 768px */
@media (max-width: 768px) {
  #custom-navbar ul {
    flex-direction: column;
  }
}

/* Tablette */
@media (max-width: 1024px) {
  .sidebar {
    display: none;
  }
}
```

> 📖 Recettes CSS avancées → `references/css-avance.md`

---

## 6. JavaScript — Intégration et bonnes pratiques

### Où ajouter du JS

- **Affichage → Gestion des codes JavaScript** (recommandé, depuis déc. 2025 : éditeur JS intégré + désactivation en 1 clic + recherche dans les JS)
- Directement dans un template (entre `<script>` et `</script>`)

### Accéder aux données utilisateur

```javascript
// _userdata est un objet global disponible sur toutes les pages AwesomeBB
console.log(_userdata); // Voir toutes les propriétés disponibles

// Exemple : afficher le pseudo dans un div custom
document.getElementById("mon-pseudo").innerHTML = _userdata.username;
```

### Cibler des éléments AwesomeBB

```javascript
// jQuery est disponible nativement sur Forumactif
$(document).ready(function () {
  // Cacher la toolbar Forumactif
  $("#page-top").hide();

  // Modifier la navbar
  $("header nav").prepend('<li><a href="/monlien">Mon lien</a></li>');
});
```

### Pagination AwesomeBB (sélecteur correct)

```javascript
// Sélecteur pagination AwesomeBB :
".pagination:not(strong)";
```

> ⚠️ Attention aux mises à jour : certains scripts peuvent casser après une MAJ de Forumactif (ex. pagination modifiée en fév. 2026). Toujours vérifier après une mise à jour.

> 📖 Scripts JS utiles → `references/javascript.md`

---

## 7. Mises à jour récentes de Forumactif (2025–2026)

| Date          | Mise à jour                                                                                |
| ------------- | ------------------------------------------------------------------------------------------ |
| **Déc. 2025** | CSS additionnel, nouveau template profil avancé, recherche dans la gestion des JS          |
| **Déc. 2025** | Nouvel éditeur JS, désactivation des JS en 1 clic, bouton Télécharger, plus de JS hébergés |
| **Déc. 2025** | Nouveau système de récompenses                                                             |
| **Déc. 2025** | Nouveaux Outils SEO                                                                        |
| **Fév. 2026** | Modification de la pagination AwesomeBB (peut casser des scripts existants)                |
| **Fév. 2026** | Optimisation affichage mobile ModernBB                                                     |

> 📡 Pour rester à jour : consulte régulièrement [forum.forumactif.com/f1-annonces-mises-a-jour](https://forum.forumactif.com/f1-annonces-mises-a-jour)

---

## 8. Gestion complète du forum — Checklist

### Administration quotidienne

- [ ] Modérer les messages (Modération → File d'attente)
- [ ] Gérer les inscriptions (Membres → Inscriptions en attente)
- [ ] Vérifier les signalements (Modération → Signalements)

### Personnalisation

- [ ] Templates modifiés dans **Affichage → Templates → Général**
- [ ] CSS dans **Affichage → Couleurs → Feuille de style CSS**
- [ ] JS dans **Affichage → Gestion des codes JavaScript**
- [ ] Navbar via **Affichage → Barre de navigation** (pour les liens simples)
- [ ] Widgets via **Affichage → Widgets**

### SEO & Performance

- [ ] Configurer les balises meta (Général → Configuration → SEO)
- [ ] Utiliser des URLs propres
- [ ] Optimiser les images (hébergées sur servimg.com ou imgbb.com)

### Sécurité

- [ ] Ne jamais partager l'accès Fondateur
- [ ] Utiliser les groupes de modération pour déléguer
- [ ] Activer la validation des inscriptions si nécessaire

---

## 9. Ressources et références

| Ressource                      | Lien                                                  |
| ------------------------------ | ----------------------------------------------------- |
| Support officiel Forumactif    | https://forum.forumactif.com                          |
| Annonces & MAJ                 | https://forum.forumactif.com/f1-annonces-mises-a-jour |
| Démo AwesomeBB                 | https://awesomebb.forumgaming.fr                      |
| CSSActif (tutos CSS/Templates) | https://css-actif.forumactif.org                      |
| Blank Theme AwesomeBB          | https://blank-theme.com                               |
| HitSkin (thèmes)               | https://www.hitskin.com                               |

---

## Références internes (lire selon le besoin)

- `references/templates.md` → Détail de chaque template, variables associées, exemples de modification
- `references/variables.md` → Liste complète des variables Forumactif utilisables dans les templates
- `references/css-avance.md` → Recettes CSS : navbar, cartes, responsive, animations
- `references/javascript.md` → Scripts JS utiles : \_userdata, manipulation DOM, intégrations
