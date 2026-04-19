# Templates AwesomeBB — Référence complète

## overall_header

Template le plus important. Contient la structure globale du haut de page.

### Variables disponibles dans overall_header

| Variable              | Description                               |
| --------------------- | ----------------------------------------- |
| `{U_INDEX}`           | URL de la page d'accueil                  |
| `{LOGO}`              | URL du logo du forum                      |
| `{L_INDEX}`           | Nom du forum (texte alternatif logo)      |
| `{SITENAME}`          | Nom du forum                              |
| `{GENERATED_NAV_BAR}` | Liens de navigation configurés dans le PA |
| `{NAVBAR_BORDERLESS}` | Classe CSS pour navbar sans bordure       |
| `{U_LOGIN_LOGOUT}`    | URL de connexion/déconnexion              |
| `{L_LOGIN_LOGOUT}`    | Texte "Connexion" ou "Déconnexion"        |
| `{U_REGISTER}`        | URL d'inscription                         |
| `{U_PROFILE}`         | URL du profil utilisateur                 |
| `{U_PRIVATEMAIL}`     | URL des messages privés                   |
| `{U_SEARCH}`          | URL de la recherche                       |
| `{U_VIEWONLINE}`      | URL des membres en ligne                  |
| `{META_DESCRIPTION}`  | Description meta pour le SEO              |
| `{PAGE_TITLE}`        | Titre de la page courante                 |
| `{T_THEME_PATH}`      | Chemin vers le dossier du thème           |
| `{T_IMAGESET_PATH}`   | Chemin vers les images du thème           |

### Structure de base AwesomeBB (overall_header simplifié)

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="{META_DESCRIPTION}" />
    <title>{PAGE_TITLE}</title>
    {CSS} {JAVASCRIPT}
  </head>
  <body id="phpbb">
    <div id="page-top">
      <header>
        <div class="wrap">
          <!-- Logo -->
          <a href="{U_INDEX}" id="logo">
            <img src="{LOGO}" alt="{L_INDEX}" />
          </a>

          <!-- Navbar principale -->
          <ul class="navbar navlinks{NAVBAR_BORDERLESS}">
            <li>{GENERATED_NAV_BAR}</li>
          </ul>

          <!-- Menu hamburger mobile (JS) -->
          <div id="mobile-menu-toggle">...</div>

          <!-- Éléments générés par JS (menu user, notifications) -->
        </div>
      </header>
    </div>

    <!-- Contenu de la page -->
    <div id="page-body"></div>
  </body>
</html>
```

### Exemple : Navbar custom complète

```html
<!-- Dans overall_header, remplace ou ajoute avant/après la navbar par défaut -->
<nav id="custom-navbar">
  <div class="nav-logo">
    <a href="{U_INDEX}">
      <img src="{LOGO}" alt="{SITENAME}" />
    </a>
  </div>
  <ul class="nav-links">
    <li><a href="{U_INDEX}">🏠 Accueil</a></li>
    <li><a href="/forum/regles">📋 Règles</a></li>
    <li><a href="/forum/presentation">👋 Présentations</a></li>
    <li><a href="{U_SEARCH}">🔍 Recherche</a></li>
    <!-- Lien conditionnel connexion/déconnexion -->
    <li><a href="{U_LOGIN_LOGOUT}">{L_LOGIN_LOGOUT}</a></li>
  </ul>
  <!-- Bouton hamburger pour mobile -->
  <button id="hamburger" onclick="toggleMenu()">☰</button>
</nav>
```

CSS associé :

```css
#custom-navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 var(--md);
  background: var(--primary);
  position: sticky;
  top: 0;
  z-index: 9999;
}
#custom-navbar .nav-links {
  display: flex;
  list-style: none;
  gap: var(--md);
  margin: 0;
  padding: 0;
}
#custom-navbar .nav-links a {
  color: #fff;
  text-decoration: none;
  padding: 8px 12px;
  border-radius: 4px;
  transition: background 0.2s;
}
#custom-navbar .nav-links a:hover {
  background: rgba(255, 255, 255, 0.2);
}
#hamburger {
  display: none;
}

@media (max-width: 768px) {
  #custom-navbar .nav-links {
    display: none;
    flex-direction: column;
  }
  #custom-navbar .nav-links.open {
    display: flex;
  }
  #hamburger {
    display: block;
    background: none;
    border: none;
    color: #fff;
    font-size: 24px;
    cursor: pointer;
  }
}
```

---

## overall_footer_begin & overall_footer_end

### Variables disponibles

| Variable                 | Description                    |
| ------------------------ | ------------------------------ |
| `{U_INDEX}`              | URL accueil                    |
| `{SITENAME}`             | Nom du forum                   |
| `{PAGE_GENERATION_TIME}` | Temps de génération de la page |
| `{SQL_TIME}`             | Temps des requêtes SQL         |

### Exemple : Footer custom

```html
<!-- Dans overall_footer_begin -->
<footer id="custom-footer">
  <div class="footer-content">
    <p>© {SITENAME} — Tous droits réservés</p>
    <nav>
      <a href="{U_INDEX}">Accueil</a>
      <a href="/forum/contact">Contact</a>
      <a href="/forum/mentions-legales">Mentions légales</a>
    </nav>
  </div>
</footer>
```

---

## index_body

Page d'accueil du forum. Contient la liste des catégories.

### Variables importantes

| Variable                  | Description                               |
| ------------------------- | ----------------------------------------- |
| `{TOTAL_POSTS}`           | Nombre total de messages                  |
| `{TOTAL_USERS}`           | Nombre total de membres                   |
| `{NEWEST_USER}`           | Dernier membre inscrit                    |
| `{RECORD_USERS}`          | Record de connectés simultanément         |
| `{giefmod_index1.MODVAR}` | Widgets latéraux gauche                   |
| `{site_widgets}`          | Widgets latéraux droite (depuis MAJ 2021) |

---

## viewtopic_body

Affichage d'un sujet.

### Variables des messages

| Variable                  | Description                    |
| ------------------------- | ------------------------------ |
| `{postrow.POSTER_NAME}`   | Pseudo de l'auteur             |
| `{postrow.POSTER_AVATAR}` | Avatar de l'auteur             |
| `{postrow.POST_DATE}`     | Date du message                |
| `{postrow.MESSAGE}`       | Contenu du message             |
| `{postrow.MINI_POST_VAR}` | Lien direct vers le message    |
| `{postrow.RANK_IMAGE}`    | Image du rang                  |
| `{postrow.POSTER_POSTS}`  | Nombre de messages de l'auteur |

---

## profile_view_body

### Variables du profil

| Variable           | Description        |
| ------------------ | ------------------ |
| `{PROFILE_AVATAR}` | Avatar du membre   |
| `{USERNAME}`       | Pseudo             |
| `{JOINED}`         | Date d'inscription |
| `{POSTS}`          | Nombre de messages |
| `{LAST_ACTIVE}`    | Dernière activité  |
| `{SIGNATURE}`      | Signature          |

---

## Blocs conditionnels (BEGIN/END)

Forumactif utilise une syntaxe de boucles dans les templates :

```html
<!-- BEGIN nom_bloc -->
<!-- Code répété pour chaque élément -->
{nom_bloc.VARIABLE}
<!-- END nom_bloc -->
```

Exemple dans `index_body` :

```html
<!-- BEGIN catrow -->
<div class="category">
  <!-- BEGIN catrow.cathead -->
  <h2>{catrow.cathead.CAT_DESC}</h2>
  <!-- END catrow.cathead -->
  <!-- BEGIN catrow.forum_row -->
  <div class="forum-row">
    <a href="{catrow.forum_row.U_VIEWFORUM}">{catrow.forum_row.FORUM_NAME}</a>
  </div>
  <!-- END catrow.forum_row -->
</div>
<!-- END catrow -->
```
